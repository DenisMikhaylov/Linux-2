Установка и настройка Http proxy

Установка 

```
gate:~# apt install squid
```

Настройка

```
gate# cat /etc/squid/conf.d/my.conf
или
gate# cat /etc/squid/squid.conf
```

Посмотреть то чт реально настроено в конфигурационном файле

```
grep -Ev '^$|^#' /etc/squid/squid.conf
```

Настроим правило
```
/etc/squid/squid.conf
```

```
...
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
...
После этого блока

#http_access allow localnet
acl our_networks src 192.168.30.0/24
acl rambler src .rambler.ru

http_access deny rambler
http_access allow our_networks
...

```

Проверка

```
squid -k check
```
Переконфигурация

```
squid -k reconfigure
```

Просмотр логов

```
ls /var/log/squid
```
