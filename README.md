# zabbix templates templates

> Справедливо для DEB и RPM дистрибутивов

> zabbix-agent должен быть установлен на наблюдаемом узле

## Apache

## Nginx

## MySQL

## Postfix
* Мониторинг очереди
* Мониторинг полученных, отправленных байт
* Мониторинг полученных, отправленных писем и других параметров

## Bind / Named



### Zabbix repository
zabbix repo DEB: `wget http://repo.zabbix.com/zabbix/2.2/debian/pool/main/z/zabbix-release/zabbix-release_2.2-1+wheezy_all.deb`
`dpkg -i zabbix-release_2.2-1+wheezy_all.deb && apt-get update`

zabbix repo RPM: `rpm -ivh http://repo.zabbix.com/zabbix/2.2/rhel/6/x86_64/zabbix-release-2.2-1.el6.noarch.rpm`

epel: `rpm -ihv http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm` или `rpm -ihv http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`

