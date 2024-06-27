Пример 
https://www.altlinux.org/FreeRADIUS

Установка
```
# apt install freeradius
```
Настройка сервера
Настройка c использованием текстовых файлов
```
# nano /etc/freeradius/3.0/clients.conf
```
```
...
client gate.corp.ru {
        secret          = testing123
        shortname       = gate
}

```

Настройка
```
# :> /etc/freeradius/3.0/users
```
```
# nano /etc/freeradius/3.0/users
```
```
user1 Cleartext-Password := "rpassword1"
user2 Cleartext-Password := "rpassword2", Simultaneous-Use := 1
student Cleartext-Password := "password"
```
или
```
user1 Cleartext-Password := "rpassword1"
     Framed-IP-Address = 192.168.10.30

user2 Cleartext-Password := "rpassword2", Simultaneous-Use := 1
     Framed-IP-Address = 192.168.10.10,
```

```
# nano /etc/freeradius/3.0/radiusd.conf
```
```
...
log {
  ...
  auth = yes
...
```
```
# nano /etc/freeradius/3.0/sites-available/default
```
```
authorize {
...
#	unix
	files
accounting {
...
	radutmp
...
session {
...
	radutmp
...
```

```
# nano /etc/freeradius/3.0/mods-available/radutmp
```
```
...
check_with_nas = no
...
```

Запуск сервера
```
# systemctl enable freeradius

# service freeradius restart
```


Тестирование сервера

```
# apt install freeradius-utils
```
```
$ radtest user1 rpassword1 127.0.0.1 0 testing123

$ echo "User-Name=student,User-Password=password,NAS-IP-Address=127.0.0.1" | radclient localhost auth testing123
```

```
# tail -f /var/log/freeradius/radius.log
```
