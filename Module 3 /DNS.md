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
применяем только при перенастройки интерфейсов
```
systemctl status bind9
```
Проверка состовляемой конфигурации

```
gate# cat /etc/bind/named.conf
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
       listen-on { 192.168.10.1; 127.0.0.1; };
       listen-on-v6 { ::; };
       

```
Проверка конфигурации

```
# named-checkconf -z
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
zone "corp.ru" {
        type master;
        file "/etc/bind/corp.ru";
};
```

создание файла zone
```
nano /etc/bind/corp.ru
```
```
$TTL    3h
@    IN  SOA gate   root.gate  2024041601 1d 12h  1w  3h
         NS  gate
         NS  srv1
         A   192.168.10.1
         MX  10  gate
gate     A   192.168.10.1
srv1     A   192.168.10.10
srv2     A   192.168.10.20

```

Проверка DNS zano

```
# named-checkconf -z
# named-checkzone corp.ru /etc/bind/corp.ru
```

Перезапуск DNS

```
# rndc reload
```

Тестирование

```
nslookup -q=SOA corp.ru
nslookup -q=A gate.corp.ru
```

Создание обратной zone
```
nano /etc/bind/named.conf.local
```
Добавить в конец файла
```
zone "10.168.192.in-addr.arpa" in {
  type master;
  file "/etc/bind/10.168.192.IN-ADDR.ARPA";
};
```
Создание файла обратной зоны
```
nano /etc/bind/10.168.192.IN-ADDR.ARPA
```
```
$TTL    3h
@ IN SOA corp.ru.  root.gate.corp.ru. 1 1d 12h  1w  3h
     NS  gate.corp.ru.

1    PTR  gate.corp.ru.
10   PTR  srv1.corp.ru.
20   PTR  srv2.corp.ru.

```
```
# named-checkconf -z

# service bind9 restart
```
