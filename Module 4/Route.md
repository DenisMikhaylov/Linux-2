Настройка маршрута

Подключаемся к gate

Настраиваем перенаправление ip
```
nano /etc/sysctl.conf
```
Правим строку
```
net.ipv4.ip_forward=1
```
Перечитаем конфигурацию

```
sysctl -f
```

Проверяем

```
sysctl net.ipv4.ip_forward
```
Проверка таблицы маршрутов

```
ip r
```

Настройка и установка ip forward

Установка iptables
```
# apt install iptables -y
```

Посмотрим правила текущие

Отобразится таблица filter
```
iptables -L
```
Отобразится таблица nat

```
iptables -t nat -L
```
Переходим на srv1

проверяем работу
```
ping -c4 ya.ru
```
ответа не будет. так как пакет не вернулся. но был отправлен.

Добавлем правило на gate

```
iptables -t nat -A POSTROUTING -o eth0 -s 192.168.10.0/24 -j MASQUERADE
```
Проверяем

```
iptables -t nat -L
```

Сохраним или просмотрим iptables

Просмотреть
```
iptables-save
```
сохранить 
```
iptables-save > testiptables
```

Переходим на srv1

проверяем работу
```
ping -c4 ya.ru
```

Переключаемся на gate

Устанавливаем пакет для загрузки настроек iptables
```
apt install iptables-persistent -y
Yes
Yes
```

Дальнейшие сохранниния можно сделать следующей командой

```
iptables-save > /etc/iptables/rules.v4
```

Блокируем внешние запросы

```
iptables -A INPUT -i eth0 -p udp --dport 53 -j DROP
```

Можно проверить теперь работу DNS коллег указывая из ip eth0 от Gate
```
nslookup -q=A corp.ru <ip address>
```
```
iptables -L
```

Сmотрим номер правило
```
iptables --line-numbers -t nat -L

```
смотрим номер правила

```
iptables -D INPUT <ввести номер правила>
```
Восстановите правило использую iptables-restore

