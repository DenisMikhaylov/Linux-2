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
       // listen-on { 192.168.30.1; 127.0.0.1; }
       listen-on-v6 { ::; };
```

```
nano /etc/bind/named.conf.local
```
```
zone "corp1.ru" {
        type slave;
        file "/var/cache/bind/corp1.ru";
        masterfile-format text;
        masters { 192.168.30.1; };
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
cat /var/cache/bind/corp1.ru
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

```
nano /etc/bind/named.conf
```
```
include "/etc/bind/named.conf.options";
view "inside" {
  match-clients {
    192.168.30/24;
    127/8;
  };
  include "/etc/bind/named.conf.local";
  include "/etc/bind/named.conf.default-zones";
};

view "outside" {
  match-clients {"any";};
  allow-recursion {"any";};
  zone "corp1.ru" {
    type master;
    file "/etc/bind/corp1.ru.out"
  };
};
```
