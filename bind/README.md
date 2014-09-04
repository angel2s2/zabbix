# zabbix templates

## Bind / Named

### Статистика   и запросы
В /etc/bind/named.conf добавить

    statistics-channels {
         inet 127.0.0.1 port 8053 allow { 127.0.0.1; };
    };

И рестартануть bind `/etc/init.d/bind9 restart`

Установить доп. пакеты: `aptitude install xml2 curl`

Проверка: `curl -s http://localhost:8053/ 2>/dev/null | xml2 | grep -A1 -E 'queries|=Qry'`

Конфиг агента `zabbix_agentd.conf.d/userparameter_bind.conf` положить в `/etc/zabbix/zabbix_agentd.conf.d/`.

Рестартануть агента `/etc/init.d/zabbix-agent restart`.

Имптортировать шаблон bind `templates/bind.xml`.

Пример использования:

    bind.queries.in[A]
    bind.queries.out[txt]
    bind.queries.query[nxdomain]

Допустимые значения (регистр не учитывается):

    Для in и out: A, AAAA, ANY, CNAME, MX, NS, PTR, SOA, SPF, TXT
    Для query: Success, AuthAns, NoauthAns, Referral, Nxrrset, SERVFAIL, FORMERR, NXDOMAIN, Recursion, Duplicate, Dropped, Failure

PS: `templates/zabbix_screens.bind.xml` - комплесные экраны.


> использованные материалы
> http://www.netmess.org/monitoring-bind9-dns-server-with-zabbix/

