# Настройка NFS

Установка NFS сервера

```
# apt install nfs-kernel-server -y
```

Настройка

```
# nano /etc/exports
```
Просмотреть настройки

Настроим доступ до каталога

```
mkdir /var/nfs
ls -la /var/nfs
```
Настроить права на запись

```
chmod 777 /var/nfs
```

Настройка доступа

```
# nano /etc/exports
```
Добавлем строку

```
/var/nfs  192.168.30.20(rw,sync,no_subtree_check)
```

Перезапускаем сервис

```
systemctl restart nfs-kernel-server
or
service nfs-kernel-server force-reload
```

Переключаемся на SRV1

Устанавливаем клиента NFS

```
# apt install nfs-common
```

Сервис в дебиане может быть быть выключен и замаскирован

```
```

