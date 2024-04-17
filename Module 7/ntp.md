Локализация временной зоны

```
$ timedatectl

$ timedatectl list-timezones

# timedatectl set-timezone Europe/Moscow
```

Установка времени

```
date +%T -s "11:14:00"

timedatectl set-time "2018-10-06 10:35:00"
```

Вывод времени в нужном формате

```
$ date

$ date -u

$ date "+%Y-%m-%d_%H:%M:%S"

$ date --date "1 day ago" +%Y-%m-%d

$ date --date "1 hour ago" "+%Y-%m-%d %H"
```
Установка и настройка ntp сервера
```
gate:~# apt install ntp
```
Настройка
```
gate# nano /etc/ntp.conf
```
```
server 0.ru.pool.ntp.org
server 1.ru.pool.ntp.org
server 2.ru.pool.ntp.org
server 3.ru.pool.ntp.org
```

Перезапуск

```
gate# service ntp restart
```

Через 5-10 минут проверяем:

```
gate# ntpq -pn
```
```
*128.0.142.251  ...
+193.192.36.3   ...
+188.225.9.167  ...
```
```
gate# watch ntptrace

gate# apt install ntpstat

gate# ntpstat
```
```
gate# ntpdate time.apple.com
```
Настройка синхронизации с сервера gate
Переходим на SRV1

Проверяем синхронизацию
```
timedatectl
```
Настройка синхронизации
```
nano /etc/systemd/timesyncd.conf
```
```
[time]
NTP=gate.corp1.ru

```
перезапуск службы

```
systemctl restart systemd-timesyncd

```
Проверяем синхронизацию
```
timedatectl
```

```
systemctl status systemd-timesyncd
```
Смотрим поле Status



