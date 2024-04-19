Установка, настройка

```
gate:~# apt install sarg
```
```
gate:~# nano /etc/sarg/sarg.conf
```
```
...
access_log /var/log/squid/access.log
...
```
```
gate:~# nano /etc/sarg/sarg-reports.conf
```
```
...
HTMLOUT=/var/www/html/sarg
...
```

Автоматизация процесса построения отчета
```
gate:~# less /etc/cron.daily/sarg
```
```
gate:~# less /etc/logrotate.d/squid
```

Построения отчета в ручном режиме
```
gate:~# /usr/sbin/sarg-reports manual 07/02/2014

gate:~# /usr/sbin/sarg-reports today
```
