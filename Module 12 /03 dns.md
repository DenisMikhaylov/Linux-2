
Ограничение доступа к DNS серверу

```
# nano /etc/bind/named.conf.options
```
```
options {
...
        allow-recursion {192.168.10/24;  127/8;};
...
};
```
Мониторинг DNS сервера

Настройка
```
# nano /etc/bind/named.conf
```
```
options {
...
        dump-file       "/tmp/named_dump.db";
        statistics-file "/tmp/named.stats";
...
};
```
Просмотр кеша DNS сервера
```
# rndc dumpdb -cache

# less /var/cache/bind/named_dump.db
```

Просмотр статистики обращений к DNS серверу
```
# rndc stats
# nano /var/named/var/stats/named.stats
success - число запросов, не вызвавших ошибок или возврата клиенту ссылки
referral - число запросов, на которые сервер вернул клиенту ссылки
nxrrset - несуществующих записей запрошенного типа для доменного имени
nxdomain - несуществующих доменных имен
recursion - число запросов, потребовавших рекурсивной обработки
failure - число ошибок, кроме nxrrset и nxdomain
```
