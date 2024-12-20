# Установка и настройка Apache2

Установка Apache2

Переходим на сервре srv1

```
# apt install apache2
```
Смотрим структуру 

```
ls -l /etc/apache2
```
```
ls -l /etc/apache2/sites-available
```
```
ls -l /etc/apache2/sites-enabled
```

Заходим в конфигурацию

```
nano /etc/apache2/sites-available/000-default.conf
```
Просмотрите файл .


Создадим свой сайт

Бекапируем файл сайта
```
cp /var/www/html/index.html /var/www/html/index.html.bak
```
```
:> /var/www/html/index.html
```

Редактируем файл

```
nano /var/www/html/index.html
```
```
<html>
<h1>Hello Student</h1>
</html>
```

Создание своего сайта

Копируем настройки
```
cp /etc/apache2/sites-available/000-default.conf  /etc/apache2/sites-available/www.conf
```

```
nano /etc/apache2/sites-available/www.conf
```
```
ServerName www.corp.ru
DocumentRoot /var/www/corp
```
Активируем сайт

```
a2ensite www
```

Создаем структуру

```
mkdir /var/www/corp
```
```
cp /var/www/html/index.html /var/www/corp
```
Редактируем файл

```
nano /var/www/corp/index.html
```
```
<html>
<h1>CORP</h1>
<h1>Hello Student</h1>
</html>
```
Перезапускаем
```
systemctl restart apache2
```
Настраиваем DNS

Переходим на gate
```
nano /etc/bind/corp.ru
или
nano /var/cache/bind/corp.ru
```
Добавляем строку и меняем серийный номер
```
202404181300
www        CNAME        srv1
```
Если у вас два сервера WEB можно сделать DNS Round robin
```
202404181300
www        CNAME        srv1
www        CNAME        srv2
```
или
```
202404181300
www        CNAME        192.168.30.20
www        CNAME        192.168.30.21
```
Проверяем DNS

```
named-checkconf -z
```
```
rndc reload
```
Проверить DHCP на DNS
```
nano /etc/dhcp/dhcpd.conf
```
```
option domain-name-servers 192.168.10.1;
```
Протестируйте в браузере на Windows
```
http://www.corp.ru
```
