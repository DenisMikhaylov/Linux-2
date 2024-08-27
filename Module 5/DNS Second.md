Настройка вторичного DNS


Подключаемся к srv1

Обновляем репозиторий

```
apt update
```

Устанавливаем DNS

```
apt install bind9 -y
```

Открываем настройки

```
nano /etc/bind/named.conf.options
```
```
forwarders {
                8.8.8.8;
        };
       dnssec-validation no;
       // listen-on { 192.168.10.1; 127.0.0.1; }
       listen-on-v6 { ::; };
```

```
nano /etc/bind/named.conf.local
```
```
zone "corp.ru" {
        type slave;
        file "/var/cache/bind/corp.ru";
        masterfile-format text;
        masters { 192.168.10.1; };
};
```

Перезапускаем и проверяем bind

```
named-checkconf
```
```
systemctl restart bind9
```
Провыеряем создание файлов

```
ls /var/cache/bind
```
Должен появиться фаил с нашей зоной

```
cat /var/cache/bind/corp.ru
```

НАстройки на себя DNS
```
nano /etc/resolv.conf
```
```
nameserver 127.0.0.1
```

Настройка DNS просмотра для внешних и внутренних клиентов.
Настройка опциональна
на gate
```
nano /etc/bind/named.conf
```
```
include "/etc/bind/named.conf.options";
view "inside" {
  match-clients {
    192.168.10/24;
    127/8;
  };
  include "/etc/bind/named.conf.local";
  include "/etc/bind/named.conf.default-zones";
};

view "outside" {
  match-clients {"any";};
  allow-recursion {"any";};
  zone "corp.ru" {
    type master;
    file "/etc/bind/corp.ru.out"
  };
};
```
```
# nano /etc/bind/corp.ru.out
```
```
$TTL    3h
corp.ru. SOA ns root.server 1 1d 12h 1w 3h
;

```
Настройка Динамического обновления

Подключение к серверу Gate

```
# nano dhcpd.conf
```
```
ddns-update-style interim;
ddns-ttl 60;

subnet 192.168.10.0 netmask 255.255.255.0 {


include "/etc/dhcp/rndc.key";
  zone corp.ru. {
    primary 192.168.10.1;
    key rndc-key;
  }
  zone 10.168.192.in-addr.arpa. {
    primary 192.168.10.1;
    key rndc-key;
  }
```
Проверка конфигурации
```
# dhcpd -t
```

Настройка DNS
```
# mv /etc/bind/corp* /var/cache/bind/
# chown -R bind /var/cache/bind/
cp /etc/bind/rndc.key /etc/dhcp/
```
```
nano  /etc/bind/named.conf.local
```
```
...
        zone "corp.ru" {
...
               allow-update { key "rndc-key"; };
        };
...
        zone "10.168.192.IN-ADDR.ARPA" {
...
               allow-update { key "rndc-key"; };
        };
...
include "/etc/bind/rndc.key";
```

Ограничение доступа к DNS серверу

```
nano /etc/bind/named.conf.options
```
Добавить настройку
```
options {
...
        allow-recursion {192.168.10/24; 127/8;};
...
}

```
Просмотр статистики обращений к DNS серверу

```
nano /etc/bind/named.conf.options
```
```
options{

  zone-statistics yes;
}
```
```
# rndc stats
# cat /var/cache/bind/named.stats
success - число запросов, не вызвавших ошибок или возврата клиенту ссылки
referral - число запросов, на которые сервер вернул клиенту ссылки
nxrrset - несуществующих записей запрошенного типа для доменного имени
nxdomain - несуществующих доменных имен
recursion - число запросов, потребовавших рекурсивной обработки
failure - число ошибок, кроме nxrrset и nxdomain
```
