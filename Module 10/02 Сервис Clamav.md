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


Журнал
```
# tail -f /var/log/clamav/clamav.log
```
