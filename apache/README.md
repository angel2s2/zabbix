# zabbix templates

## Apache

### Настройка apache
В http.conf добавить

    LoadModule status_module modules/mod_status.so
    ExtendedStatus On
    <Location /server-status>
        SetHandler server-status
        Order Deny,Allow
        Deny from all
        Allow from 127.0.0.1
    </Location>

И рестартануть апач `/etc/init.d/apache2 restart`
Проверить как работает `wget -qO- 'http://localhost:81/server-status?auto'`. Если ничего не возвращает, возможно rewrite сайта по умолчанию не пускает... Чтобы исправить нужно в начало .htaccess сайта по умолчанию добавить:

    <IfModule mod_rewrite.c>
      RewriteCond %{REQUEST_URI} !=/server-status
      RewriteCond %{REQUEST_URI} !=/server-info
    </IfModule>

Положить скрипт `scripts/apache.sh` в `/etc/zabbix/scripts/apache.sh` и сделать исполняемым `chmod ugo+x /etc/zabbix/scripts/apache.sh`. Если апач НЕ на 80 порту, нужно подравить переменную `APACHE_STATS_DEFAULT_URL`.
Проверить можно так: `/etc/zabbix/scripts/apache.sh total_kbytes`. Если вернет отрицательное значение, типа `-0.990`, значит где-то допустил ошибку.

Конфиг агента `zabbix_agentd.conf.d/userparameter_apache.conf` положить в `/etc/zabbix/zabbix_agentd.conf.d/userparameter_apache.conf`.

Рестартануть агента `/etc/init.d/zabbix-agent restart`.

Имптортировать шаблон апача `templates/apache.xml`.

> использованные материалы
> http://www.klipach.com/apach-monitoring-in-zabbix/
