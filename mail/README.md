# zabbix templates

## mail

### Courier-imap

Мониторинг изменений конфигов. 
Мониторинг лога courier-imap на наличие превышения количества подключений. Параметры в конфиге courier-imap MAXDAEMONS и MAXPERIP.
// используется как пассивная, так и активная проверка


### Other

Мониторинг изменений конфигов разных программ. Например, spamassassin, clamav, amavisd, authlib, sasl2  и других.




### Примечание

В конфиге zabbix-агента `/etc/zabbix/zabbix_agentd.conf` прописать:
    
    ServerActive=<ip zabbix-сервера>
    Hostname=<имя наблюдаемого узла сети (зарегистрированное в веб-интерфейсе Zabbix)>


Импортировать шаблон `templates/courier-imap.xml` Настройка -> Шаблоны -> Импорт и добавить в шаблон "Узлы сети".

PS: Если mail в DMZ, а zabbix нет, не забыть сделать проброс порта mail -> zabbix:10051


