Управление доступом к HTTP серверу
```
Nano /etc/apache2/sites-available/000-default*
```
```

        <Directory /var/www/html>
                #Order Deny,Allow
                #Deny from all
                #Allow from 192.168.X.0/24
                #Allow from 127.0.0.1
                Require ip 127.0.0.1 192.168.10.0/24
        </Directory>
```
