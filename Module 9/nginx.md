# Установка Nginx

Установка Nginx на gate

```
gate# apt install nginx
```

Смотрим базовые настройки

```
# ls /etc/nginx/sites-available
```

Настройка виртуальных хостов

```
mkdir -p /var/www/example_1.ru/html
```

Создание индексного файла

```
nano /var/www/example_1.ru/html/index.html
```
```
<html>
    <head>
        <title>Example 1!</title>
    </head>
    <body>
        <h1>Success! 1st server block is working!</h1>
    </body>
</html>
```

Создание виртуальных хостов

```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example_1.ru
```
Редактируем настройки
```
nano /etc/nginx/sites-available/example_1.ru
```

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/example_1.ru/html;
        index index.html index.htm index.nginx-debian.html;

        server_name www.corp1.ru;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Подключение виртуальных хостов

```
ln -s /etc/nginx/sites-available/example_1.ru /etc/nginx/sites-enabled/
```

Редактируем файл NGINX

```
nano /etc/nginx/nginx.con
```
```
server_names_hash_bucket_size
```

Проверяем настройки

```
nginx -t
```
Перезагрузим наш веб-сервер

```
systemctl restart nginx
```
Настраиваем DNS

Переходим на gate
```
nano /etc/bind/corp1.ru
```
Добавляем строку и меняем серийный номер
```
202404181300
www        CNAME        gate
```
Проверяем DNS

```
named-checkconf -z
```
```
rndc reload
```
