Установка
```
# apt install freeradius
```
Настройка сервера
Настройка c использованием текстовых файлов
```
# cat /etc/freeradius/3.0/clients.conf
```
```
...
client gate.corp1.ru {
        secret          = testing123
        shortname       = gate
}

client switch {
       secret          = testing123
       shortname       = switch
}
```
```
#client switch1 { secret = testing123 }
#client switch2 { secret = testing123 }
#client switch3 { secret = testing123 }
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
```
# cat /etc/freeradius/3.0/radiusd.conf
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
