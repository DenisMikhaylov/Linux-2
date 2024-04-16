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

