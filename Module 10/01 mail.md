# Настройка почтовой системы.


Подклюбчаемся к компьютеру Gate

Установка postfix

```
# apt install postfix -y
```
```
Internet sile
System mail name = corp.ru
```

Проверим работу

```
telnet 127.0.0.1 25
```
```
HELO gate.corp.ru
MAIL FROM: root@corp.ru
RCPT TO: student@corp.ru
RCPT TO: mikhaylovd@outlook.com
DATA
Hello
.
quit
```

Проверим логи

```
nano /var/log/mail.log
```
Посмотреть данные

Создаем пользователя

```
useradd -m ivan
useradd -m anna
```
```
passwd ivan
passwd anna
```

Просмотрим файл postfix
```
nano /etc/postfix/main.cf
```
Проверим работу

```
telnet 127.0.0.1 25
```
```
HELO gate.corp.ru
MAIL FROM: root@gate.corp.ru
RCPT TO: ivan@corp.ru
DATA
Hello
.
quit
```
Смотрим отправленное письмо
```
ls /var/spool/mail
```
```
nano /var/spool/mail/ivan
```
Почтовые псевдонимы (группы рассылки)

```
nano /etc/aliases
```
```
hr: ivan,anna
```
генерируем базу данных

```
newaliases
```

Установка dovecot

```
apt install dovecot-imapd -y
```

Основной файл конфигурации

```
nano /etc/dovecot/dovecot.conf
```

Дополнительные файлы конфигурации

```
ls /etc/dovecot/conf.d
```
Правим параметры аутификации

```
nano /etc/dovecot/conf.d/10-auth.conf
```
```
disable_plaintext_auth = no
```
Проверка 

```
dovecot -n
```

```
nano /etc/dovecot/conf.d/10-ssl.conf
```
```
ssl = no 
```
Проверка 

```
# dovecot -n

# service dovecot restart
```

Переключаемся на Windows 
Устанавливаем mozilla thunderbird
Вводим параметры подключения
```
ivan@corp.ru
ваш пароль
```
Проверяем настройки
```
manual settings
```
Отправьте сообщение на алиас hr@corp.ru

Переключаемся на gate
```
ls /var/spool/mail
```

Настройка Web интерфейса Roundcube

```
apt install mariadb-server -y
apt install roundcube roundcube-mysql -y
```
```
yes
пароль Pa$$w0rd
```

Настройка
```
nano /etc/roundcube/config/config.inc.php
или
nano /var/lib/roundcube/config/config.inc.php
```
```
$config['default_host'] = 'localhost';
$config['smtp_host'] = 'localhost:25';
$config['smtp_user'] = '';
$config['smtp_pass'] = '';
$rcmail_config['mail_domain'] = 'corp.ru';

```
```
nano /etc/apache2/conf-enabled/roundcube.conf
```
```
убрать комментарий перед строкой Alias
```

перезапускаем apache2

```
systemctl restart apache2
```
переходим на Windows 

```
Открываем проводник
Gate.corp.ru/roundcube
пользователь ivan
```

