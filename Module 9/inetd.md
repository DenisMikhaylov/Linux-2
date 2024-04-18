Настройка Web на Srv2

Инсталировать
```
# apt install inetutils-inetd
```
Отредавтировать файл конфигурации
```
# nano /etc/inetd.conf

www stream tcp nowait root /usr/local/sbin/webd webd
```
Запустить
```
# service inetutils-inetd restart
```
создание скрипта сервиса
```
# nano /usr/local/sbin/webd
```
вставить
```
#!/bin/bash
base=/var/www
#log=/var/log/webd.log

read request
##echo "$request" >> $log       # for educational demonstration

filename="${request#GET }"
filename="${filename% HTTP/*}"

test $filename = "/" && filename="/index.html"

filename="$base$filename"

while :
do
  read -r header
##  echo "$header" >> $log       # for educational demonstration
  [ "$header" == $'\r' ] && break;
##  [ "$header" == $'' ] && break;    # for STDIN/STDOUT educational demonstration
done

if [ -e "$filename" ]
then
#  echo `date` OK $filename on `hostname` >> $log
  echo -e "HTTP/1.1 200 OK\r"
  echo -e "Content-Type: $(/usr/bin/file -bi \"$filename\")\r"
  echo -e "\r"
  /bin/cat "$filename"
else
#  echo "$(date)" ERR $filename on "$(hostname)" >> $log
  echo -e "HTTP/1.1 404 Not Found\r"
  echo -e "Content-Type: text/html;\r"
  echo -e "\r"
  echo -e "<h1>File $filename Not Found</h1>"
#  ip=$(awk '/32 host/ { print f } {f=$2}' /proc/net/fib_trie | sort -u | grep -v 127.0.0.1)
#  echo -e "Host: $(hostname), IP: $ip, ver 1.1"
fi
```
выдать права
```
# chmod +x /usr/local/sbin/webd
```

Ресурсы Web сервера на shell
```
# mkdir /var/www
```
```
# nano /var/www/index.html
```
```
<html>
<h1>Hello Student</h1>
</html>
```
Настраиваем DNS

Переходим на gate
```
nano /etc/bind/corp1.ru
```
Добавляем строку и меняем серийный номер
```
202404181300
www2        CNAME        srv2
```
Проверяем DNS

```
named-checkconf -z
```
```
rndc reload
```


Тестирйем на Windows в браузере
http://srv2.corp.ru

