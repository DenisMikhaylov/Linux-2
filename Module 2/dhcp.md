Устновка DHCP

Шаг 1. Установка ПО

Залогиниться на Gate

Установка DHCP
```
gate:~# apt install isc-dhcp-server

```
Настройка Интерфейса

```
gate:~# nano /etc/default/isc-dhcp-server
```
```
INTERFACESv4="eth1"
```

Настройка Scope

```
gate# nano /etc/dhcp/dhcpd.conf

```
```
log-facility local7;

shared-network LAN1 {
  subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.101 192.168.10.128;
    option routers 192.168.10.1;
    option domain-name "corp.ru";
    option domain-name-servers 192.168.10.1, 192.168.10.10;
    default-lease-time 600;
    max-lease-time 7200;
  }
}
```

Проверка DHCP

```
# dhcpd -t
```

Перезапуск службы DHCP

```
# service isc-dhcp-server restart

# service isc-dhcp-server status
```
Мониторинг работы DHCP

```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# grep dhcp /var/log/syslog
```

Статитска работы 
```
# apt install dhcpd-pools
# dhcpd-pools
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf

```

Создание резервации

Открыть файл конфигурации 
```
gate# nano /etc/dhcp/dhcpd.conf
```
Добавить в конец файла
Узнать mac адресс windows клиента
```
host Base18B {
hardware ethernet 00:1F:6A:21:71:3F;
fixed-address 192.168.10.101;
}
```

Перезапуск службы DHCP

```
# service isc-dhcp-server restart

# service isc-dhcp-server status
```

Настройка логирования

Установка ПО rsyslog

```
apt install rsyslog -y
```
Настройка rsyslog
```
nano /etc/rsyslog.conf
```
Доавить в Rules строку

```
Local7.*        /var/log/dhcpd.log
```

Перезапуск rsyslog

```
systemctl restart rsyslog
```

Переключиться на Windows client виртуальная машина
переключить сетевой адаптер на DHCP

Переключиться на Gate и проверить статистику повторно

Статитска работы 
```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf

```

Просмотр log

```
cat /var/log/dhcpd.log
```

Проверка подключений
```
apt install net-tools
```
```
gate# netstat -tupna4
```
