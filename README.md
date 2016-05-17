# zabbix templates 

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

## APC Symmetra LX 16000

## APC NetBotz Rack Monitor 200


### Zabbix repository
##### 2.2
zabbix repo DEB:
    deb http://repo.zabbix.com/zabbix/2.2/debian wheezy main
    deb-src http://repo.zabbix.com/zabbix/2.2/debian wheezy main
    wget -qO- http://repo.zabbix.com/zabbix-official-repo.key | apt-key add -

zabbix repo RPM: `rpm -ivh http://repo.zabbix.com/zabbix/2.2/rhel/6/x86_64/zabbix-release-2.2-1.el6.noarch.rpm`

epel: `rpm -ihv http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm` или `rpm -ihv http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`

##### 3.0
    echo 'deb http://repo.zabbix.com/zabbix/3.0/debian wheezy main' > /etc/apt/sources.list.d/zabbix.list
    echo 'deb-src http://repo.zabbix.com/zabbix/3.0/debian wheezy main' > /etc/apt/sources.list.d/zabbix.list
    wget -qO- http://repo.zabbix.com/zabbix-official-repo.key | apt-key add -


### Переименование шаблона до импорта
Открыть `xml`-файл шаблона, найти в начале файле (обычно первые 10-15 строка) строку вида `<template>Template_Name</template>` и далее сделать поиск по всему файлу по подстроке `Template_Name` и заменить на, например, `Template_Name_2`. После чего можно импортировать "переименованный" шаблон, отредактировать в интерфейсе заббикса и подключить к нужным узлам/шаблонам.


