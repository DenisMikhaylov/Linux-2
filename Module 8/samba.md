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
   guest ok = Yes
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
может не подключится
