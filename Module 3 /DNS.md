Диагности DNS

```
nslookup -q=A ya.ru
nslookup -q=AAAA ya.ru
nslookup -q=MX ya.ru
nslookup -q=TXT ya.ru
nslookup -q=SOA ya.ru
nslookup -q=NS ya.ru
```


Настройка DNS на Gate

Настройка праймери DNS сервиса

Устанвока DNS
```
gate:~# apt install bind9 -y

```
Проверка сервиса
```
systemctl status bind9
```
Проверка состовляемой конфигурации

```
Cat /etc/bind/named.conf
```

Настройка сервера перенаправляющего запросы на DNS cервер провайдера

```
# nano /etc/bind/named.conf.options
```
```
        forwarders {
                8.8.8.8;
        };
       dnssec-validation no;
       listen-on-v6 {::;}

```
Проверка конфигурации

```
# named-checkconf
```

Рестарт службы DNS

```
systemctl restart bind9
```
Тестирование пересылки

```
nslookup -1=A ya.ru 127.0.0.1
```

Правим настройки DNS
```
nano /etc/resolv.conf
```
Заменяем
```
nameserver 127.0.0.1
```

Настройка Мастер сервера

```
nano /etc/bind/named.conf.local
```
```
zone "corp1.ru" {
        type master;
        file "/etc/bind/corp1.ru";
};
```

создание файла zone
```
nano /etc/bind/corp1.ru
```
```
$TTL    3h
@    IN  SOA gate   root.gate  2024041601 1d 12h  1w  3h
         NS  gate
         NS  srv1
         A   192.168.30.1
         MX  10  gate
gate     A   192.168.30.1
srv1     A   192.168.30.20
srv2     A   192.168.30.21

```

Проверка DNS zano

```
# named-checkconf -z
# named-checkzone corp1.ru /etc/bind/corp1.ru
```

Перезапуск DNS

```
# rndc reload
```

Тестирование

```
nslookup -q=SOA corp1.ru
nslookup -q=A gate.corp1.ru
```

Создание обратной zone

```
nano /etc/bind/30.168.192.IN-ADDR.ARPA
```
```
$TTL    3h
@  SOA gate.corp1.ru  root.gate.corp1.ru. 1 1d 12h  1w  3h
         NS  gate.corp1.ru
1    PTR  gate.corp1.ru
20   PRT  srv1.corp1.ru
21   PRT  srv2.corp1.ru

```
