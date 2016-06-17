# Syslog

Логи пересылаются на rsyslog-сервер (он же zabbix-сервер). Скрипт привязывает пришедшие сообщения к соответствующим узлам сети. Т.о. триггеры создаются для нужного узла сети, а не для zabbix-сервера.
***!!! ВАЖНО !!!*** *На большом кол-ве логов и/или устройств, можно положить zabbix-сервер (либо существенно повлиять на его производительность).*

*Все настройки приведены на примере debian.*

### Настройка rsyslog-сервера
В `/etc/rsyslog.conf` раскомментировать строки:

    $ModLoad imudp
    $UDPServerRun 514

Создать файл `/etc/rsyslog.d/remote_logs.conf` следующего содержания:

    # add template for network devices
    $template network-fmt,"%TIMESTAMP:::date-rfc3339% [%fromhost-ip%] %pri-text% %syslogtag%%msg%\n"
    
    # exclude unwanted messages:
    :msg, contains, "System: Configuration change." ~
    :msg, contains, "System: Web user" ~
    :msg, contains, "System: Detected an unauthorized user attempting to access the SNMP interface from" ~
    #:msg, contains, "System: Network service started." ~
    #:msg, contains, "System: Network Interface restarted." ~
    #:msg, contains, "System: NTP update successful." ~
    #:msg, contains, "" ~

    # action for every message:
    #пример, как можно логи не только zabbix отдавать, но и в файлы записывать
    #if $syslogfacility-text == 'local4' and $fromhost-ip == '10.10.10.1' then /var/log/remote_logs/device1.log;network-fmt
    #if $syslogfacility-text == 'local4' and $fromhost-ip == '10.10.10.2' then /var/log/remote_logs/device2.log;network-fmt
    if $fromhost-ip != '127.0.0.1' and $syslogfacility-text == 'local4' then ^/etc/zabbix/extensionscripts/zabbix_syslog_lkp_host.pl;network-fmt
    & ~

В этом файле `local4` во всех `syslogfacility-text` нужно заменить на тот facility, который указан на устройстве. Либо на самом устройстве указать facility равным local4. Либо и вовсе убрать фильтрацию по facility.

Перезапустить `rsyslog`, чтобы изменения вступили в силу.

### Настройка zabbix
Импортировать шаблон `templates/zabbix_template.syslog.xml` и прикрепить его к узлам сети, с которых нужно собирать syslog-сообщения. 
***!!! ВАЖНО !!!*** *В интерфейсах узла сети указать те IP-адреса, с которых будут приходить syslog-сообщения в zabbix, иначе не получится идентифицировать источник.**

Содать пользователя, например, `api_user` типа `Zabbix Super Administrator`, т.е. с правом записи в узлы сети. Я его так же поместил в группу `No access to the frontend`.

Закинуть на zabbix-сервер файлы `extensionscripts/zabbix_syslog_lkp_host.pl` и `extensionscripts/zabbix_syslog_lkp_host.cfg`. Далее:

    mkdir /etc/zabbix/extensionscripts
    mv zabbix_syslog_lkp_host.pl zabbix_syslog_lkp_host.cfg /etc/zabbix/extensionscripts/
    chmod +x /etc/zabbix/extensionscripts/zabbix_syslog_lkp_host.pl
    chmod 600 /etc/zabbix/extensionscripts/zabbix_syslog_lkp_host.cfg
    chown zabbix:zabbix /etc/zabbix/extensionscripts/zabbix_syslog_lkp_host.cfg
    PERL_MM_USE_DEFAULT=1 perl -MCPAN -e 'install Readonly'
    PERL_MM_USE_DEFAULT=1 perl -MCPAN -e 'install CHI'
    PERL_MM_USE_DEFAULT=1 perl -MCPAN -e 'install JSON::RPC::Legacy::Client'
    PERL_MM_USE_DEFAULT=1 perl -MCPAN -e 'install Config::General'

Подправить конфиг `/etc/zabbix/extensionscripts/zabbix_syslog_lkp_host.cfg`. В большинстве случаев достаточно изменить только `password` и `user` (если это не `api_user`).

## Автоматическое крепление к карте
Можно сделать, чтобы при клике по узлу сети на карте в меню был пункт `Syslog`, позволяющий быстро перейти в `Последние данные -> нужный узел сети -> Syslog`, т.е. посмотреть полученные syslog-сообщения.

Закинуть на zabbix-сервер файл `extensionscripts/zabbix_syslog_create_urls.pl`. Далеe:

    mv zabbix_syslog_create_urls.pl /etc/zabbix/extensionscripts/
    chmod +x /etc/zabbix/extensionscripts/zabbix_syslog_create_urls.pl
    echo '*  1    * * *   zabbix /etc/zabbix/extensionscripts/zabbix_syslog_create_urls.pl' >> /etc/crontab

-----

#### Примеры настройки syslog-клиентов

##### **Linux (rsyslog)**
В `/etc/rsyslog.conf` добавить строку:

    # отправлять от любого источника (*) с любым приоритетом (*) по UDP на 10.0.0.1 
    *.*   @10.0.0.1

Вместо `*.*` можно задать фильтрацию по источнику и приоритету. Если не знаешь как, смотри, например, [тут](http://www.k-max.name/linux/syslogd-and-logrotate/).

Перезапустить `rsyslog`, чтобы изменения вступили в силу.

##### **VMWare ESXi 5+**
В `Configuration -> Advanced Settings -> Syslog -> global -> Syslog.global.logHost` указать адрес rsyslog-сервера.
В `Configuration -> Security Profile -> Firewall -> Properties... -> syslog` поставить галочку.
Вроде, нельзя задать facility и настроить фильтр. Глубоко не копал. Не использую.

##### **Symmetra LX**
В `Logs -> Syslog -> servers -> Add Server` указать адрес rsyslog-сервера, порт, язык и протокол.
В `Logs -> Syslog -> settings` поставить галку `Message Generation`, в `Facility Code` выбрать источник, например, local4.

src https://habrahabr.ru/company/zabbix/blog/252915/
