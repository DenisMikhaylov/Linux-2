Настройка Samba

Установка SAMBA
```
# apt install samba
```
Создаем полльзователя для доступа к папкам

```
useradd smbuser -s /usr/sbin/nologin
passwd smbuser
or
adduser
```
пароль Pa$$w0rd

Создаем пользователя для samba
```
smbpasswd -a smbuser
```
пароль Pa$$w0rd


Редактируем настройки

```
nano /etc/samba/smb.conf
```
добавляем в конец
```
[share1]
   path = /var/samba
   guest ok = yes
   read only = no
   force user = smbuser
```
Создаем каталог

```
mkdir /var/samba
chown smbuser.smbuser /var/samba
ls -la /var/samba
```
Проверяем 

```
testparm
```
наживаем enter
и смотрим настройки

Проверяем
переходим на Windows 

```
//192.168.30.1/share1
```

Можно с хоста по внешнему ip gate

может не подключится


Создаем папку не для всех пользователей

Редактируем настройки

```
nano /etc/samba/smb.conf
```
добавляем в конец
```
[share2]
   path = /var/share2
   guest ok = no
   read only = no
   # valid user - student как пример ограничение доступа по пользователям
```
Создаем каталог

```
mkdir /var/share2
chown smbuser.smbuser /var/share2
ls -la /var/share2
```

Проверяем 

```
testparm
```
наживаем enter
и смотрим настройки

Проверяем
переходим на Windows 

```
//192.168.30.1/share2
```

Можно с хоста по внешнему ip gate

Помощь

```
man 5 smb.conf
```

может не подключится
