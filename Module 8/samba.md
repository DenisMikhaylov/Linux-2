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
useradd Administrator
useradd student -m
```
пароль Pa$$w0rd

Создаем пользователя для samba
```
smbpasswd -a smbuser
smbpasswd -a Administrator
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
//192.168.10.1/share1
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
[global]
   guest account = nobody
[share2]
   path = /var/share2
   guest ok = yes
   public = yes
   writable = yes
   available = yes
```
Создаем каталог

```
mkdir /var/share2
chown nobody:nogroup /var/share2
chmod 0775 /var/share2
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
//192.168.10.1/share2
```

Можно с хоста по внешнему ip gate

Помощь

```
man 5 smb.conf
```

может не подключится
Открываем в Microsoft powershell

```
net use * \\192.168.10.1\share1 /USER:smbuser
```


Сокрытие названия/версии сервиса
```
# nano /etc/samba/smb.conf
```
```
[global]
...
  server string = MS File Server

...
```

Переключаемся на SRV1
Автоматическое монтирование Samba на linux

Устанавливаем Cifs

```
apt install samba-client cifs-utils -y
```

редактируем Fstab

```
nano /etc/fstab
```

```
//gate.corp.ru/share1 /home/student/samba cifs defaults,user=smbuser,noauto 0 0
```
Создается пользователя student и каталог /home/student/samba

```
mount /home/student/samba
```


