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
