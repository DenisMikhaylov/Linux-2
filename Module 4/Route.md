Настройка маршрута

Подключаемся к gate

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

