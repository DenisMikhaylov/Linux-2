# Установка, настройка и запуск pppoed

Установка
```
gate:~# apt install pppoe
```
```
gate:~# modprobe pppoe
```
Настройка

```
gate:~# nano /etc/modules
```
```
pppoe
```
```
@gate:~# nano /etc/ppp/pppoe-server-options
```
```
+chap
# ms-dns 192.168.X.10
```
```
gate:~# pppoe-server -I eth0

или

root@gate:~# pppoe-server -I eth0 -R 192.168.30.1
```


Мониторинг pppoed
```
gate:~# tail -f /var/log/syslog
```