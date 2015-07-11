# zabbix templates

## Postfix

### Мониторинг очереди
На сервере postfix создать файл `/etc/zabbix/zabbix_agentd.d/userparameter_postfix.conf` с текстом:

    UserParameter=postfix.queue,   mailq | awk '/Request./ {print $5}'
    UserParameter=postfix.maildrop,find /var/spool/postfix/maildrop -type f | wc -l
    UserParameter=postfix.deferred,find /var/spool/postfix/deferred -type f | wc -l
    UserParameter=postfix.incoming,find /var/spool/postfix/incoming -type f | wc -l
    UserParameter=postfix.active,  find /var/spool/postfix/ative -type f | wc -l

### Мониторинг полученных, отправленных байт, писем и других параметров
На сервере postfix нужно установить `zabbix-sender pflogsumm logtail`

DEB: 

    aptitude install zabbix-sender pflogsumm logtail

RPM: logtail не нашел... взял файл /usr/sbin/logtail из Debian 7

    yum install zabbix-sender postfix-perl-scripts

Скрипт `scripts/postfix.sh` положить в `/etc/zabbix/scripts/postfix.sh` и сделать исполняемым:

    chmod +x /etc/zabbix/scripts/postfix.sh

Добавить это скрипт в crontab

    echo '* * * * * root   /etc/zabbix/scripts/postfix.sh' >> /etc/crontab

В конфиге zabbix-агента `/etc/zabbix/zabbix_agentd.conf` прописать:
    
    ServerActive=<ip zabbix-сервера>
    Hostname=<имя наблюдаемого узла сети (зарегистрированное в веб-интерфейсе Zabbix)>


Импортировать шаблон `templates/postfix.xml` Настройка -> Шаблоны -> Импорт и добавить в шаблон "Узлы сети".

PS: Если postfix в DMZ, а zabbix нет, не забыть сделать проброс порта postfix -> zabbix:10050

PPS: `templates/zabbix_screens.postfix.xml` - комплексные экраны.


> испльзованные материалы
> + http://www.klipach.com/postfix-monitoring-with-zabbix/
> + http://middleswarth.net/content/adding-postfix-template-zabbix-20


