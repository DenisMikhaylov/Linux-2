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

```



Базовая конфигурация

Управление кодировкой

```
# nano /etc/apache2/sites-available/000-default.conf
```
```
...
        AddDefaultCharset utf-8
...
```
