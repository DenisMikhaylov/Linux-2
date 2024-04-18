# Установка и настройка Apache2

Установка Apache2

Переходим на сервре srv1

```
# apt install apache2
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
