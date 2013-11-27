# zabbix

## Postfix

### Мониторинг очереди
На сервере postfix создать файл `/etc/zabbix/zabbix_agentd.d/userparameter_postfix.conf` с текстом:

    UserParameter=postfix.queue,   mailq | awk '/Request./ {print $5}'
    UserParameter=postfix.maildrop,find /var/spool/postfix/maildrop -type f | wc -l
    UserParameter=postfix.deferred,find /var/spool/postfix/deferred -type f | wc -l
    UserParameter=postfix.incoming,find /var/spool/postfix/incoming -type f | wc -l
    UserParameter=postfix.active,  find /var/spool/postfix/ative -type f | wc -l

### Мониторинг полученных, отправленных писем и других параметров
На сервере postfix нужно установить `zabbix-sender pflogsumm logtail`

DEB: 

    aptitude install zabbix-sender pflogsumm logtail

RPM: 

    yum install zabbix-sender postfix-perl-scripts
    cd /tmp
    wget http://www.fourmilab.ch/webtools/logtail/logtail.tar.gz
    tar -xzf logtail.tar.gz
    cp -i logtail.pl /usr/sbin/logtail
    chmod +x /usr/sbin/logtail
    rm -f /tmp/logtail.*
    cd -
    sed -i -e 's/#! \/bin\/perl/#! \/usr\/bin\/perl/' /usr/sbin/logtail

Создать файл `/etc/zabbix/scripts/postfix.sh` (см. файл `scripts/postfix.sh`)

    mkdir -p /etc/zabbix/scripts
    touch /etc/zabbix/scripts/postfix.sh
    chmod +x /etc/zabbix/scripts/postfix.sh

С кодом

    #!/bin/bash
    
    MAILLOG=/var/log/maillog
    DAT1=/tmp/zabbix-postfix-offset.dat
    DAT2=$(mktemp)
    PFLOGSUMM=/usr/sbin/pflogsumm
    ZABBIX_CONF=/etc/zabbix/zabbix_agentd.conf
    DEBUG=0
    
    function zsend {
      key="postfix[`echo "$1" | tr ' -' '_' | tr '[A-Z]' '[a-z]' | tr -cd [a-z_]`]"
      value=`grep -m 1 "$1" $DAT2 | awk '{print $1}'`
      [ ${DEBUG} -ne 0 ] && echo "Send key "${key}" with value "${value}"" >&2
      /usr/bin/zabbix_sender -c $ZABBIX_CONF -k "${key}" -o "${value}" 2>&1 >/dev/null
    }
    
    /usr/sbin/logtail -f$MAILLOG -o$DAT1 | $PFLOGSUMM -h 0 -u 0 --no_bounce_detail --no_deferral_detail --no_reject_detail --no_no_msg_size --no_smtpd_warnings > $DAT2
    
    zsend received
    zsend delivered
    zsend forwarded
    zsend deferred
    zsend bounced
    zsend rejected
    zsend held
    zsend discarded
    zsend "reject warnings"
    zsend "bytes received"
    zsend "bytes delivered"
    zsend senders
    zsend recipients
    
    rm $DAT2

Добавить это скрипт в crontab

    echo '* * * * * root   /etc/zabbix/scripts/postfix.sh' >> /etc/crontab

Импортировать шаблон `templates/postfix.xml` Настройка -> Шаблоны -> Импорт и добавить в шаблон "Узлы сети".

> испльзованные материалы
> + http://www.klipach.com/postfix-monitoring-with-zabbix/
> + http://middleswarth.net/content/adding-postfix-template-zabbix-20


