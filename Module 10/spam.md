
Защита почты от спама

Установка
```
gate# apt install spamassassin
```

Настройка 
```
# nano /etc/spamassassin/local.cf
```
```
rewrite_header Subject *****SPAM*****
report_safe 0                        
use_bayes 0
# required_score 5.0
trusted_networks 192.168.10        # must be set for cgpav because default ALL_TRUSTED !!!
add_header all Report _REPORT_
score BODY_SINGLE_WORD 10.0
```

Проверка 
```
gate# spamassassin --lint     
```
```
gate# sa-update

```

Запуск
```
root@gate:~# nano /etc/default/spamassassin
```
```
CRON=1
```
```
gate:~# systemctl enable spamassassin

gate:~# service spamassassin start
```

Подключение SpamAssassin через milter интерфейс

Postfix 
```
gate:~# apt install spamass-milter

gate:~# less /etc/default/spamass-milter

gate:~# nano /etc/postfix/main.cf
```
```
smtpd_milters = unix:/clamav/clamav-milter.ctl unix:/spamass/spamass.sock
```
```
gate:~# service postfix restart
```

Технология Grey List

Postfix 

```
gate:~# apt install postgrey
```
```
gate:~# less /etc/default/postgrey
```
```
gate:~# nano /etc/postfix/main.cf
```
```
smtpd_recipient_restrictions = permit_mynetworks,
                reject_unauth_destination,
                check_policy_service inet:127.0.0.1:10023
```
```
gate:~# service postfix restart
```
```
gate:~# ls /var/lib/postgrey/
```
```
gate:~# postgreyreport < /var/log/mail.log
```
