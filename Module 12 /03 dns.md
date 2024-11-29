
Ограничение доступа к DNS серверу

```
# nano /etc/bind/named.conf.options
```
```
options {
...
        allow-recursion {192.168.10/24;  127/8;};
...
};
```
