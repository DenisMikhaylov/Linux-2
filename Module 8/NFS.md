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
ls -l /lib/systemd/system/nfs-common.service
```
Проверяем что есть /dev/null

Удалем файл
```
rm /lib/systemd/system/nfs-common.service
```
Запускаем сервис

```
systemctl enable nfs-common
systemctl start nfs-common
```
Смотрим доступные ресурсы

```
showmount -e gate.corp1.ru
```
Монтирование каталога
```
# mkdir /mnt/nfs
# mount gate.corp1.ru:/var/nfs /mnt/nfs
```
Добавим в автозагрузку
```
# nano /etc/fstab
```
```
192.168.30.1:/var/nfs  /mnt/nfs nfs4 defaults 0 0 
```

Примонтируем
```
mount /mnt/nfs
```

Для правильной настройки лучше использовать autofs

```
# apt install autofs
```
Редактируем файл

```
nano /etc/auto.tab
```
```
public	-ro		192.168.30.1:/var/nfs
```
Смотрим настройки автомонтирования

```
nano /etc/auto.master
```


