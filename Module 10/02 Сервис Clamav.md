# Установка Сервиса Clamav

Установка

```
apt install clamav-daemon
```

Настройка
```
rm /var/lib/clamav/freshclam.dat
nano /etc/clamav/freshclam.conf
```
```
gate:~# nano /etc/clamav/clamd.conf
```
```
...
MilterSocket /var/spool/postfix/clamav/clamav-milter.ctl
...
MilterSocketGroup postfix
...
```
```
gate:~# service clamav-daemon
```
```
root@gate:~# cat /etc/postfix/main.cf
```
```
...
milter_default_action = accept
smtpd_milters = unix:/clamav/clamav-milter.ctl
```
```
root@gate:~# service postfix reload
```


Журнал
```
# tail -f /var/log/clamav/clamav.log
```
