# zabbix templates 

> Справедливо для DEB и RPM дистрибутивов

> zabbix-agent должен быть установлен на наблюдаемом узле

### Apache
* Забирает статистику у mod_status

### bareos
* Запущены ли службы
* Открыты ли порты

### bind/named
* Входящие и исходящие запросы (A, CNAME, MX, etc)
* Статистика запросов (Success, SERVFAIL, NXDOMAIN, etc)

### linux
* Стандартные шаблоны с добавленными элементами данных и триггерами для мониторинга текущего времени на серверах.

### mail
* Изменения конфигов courier-imap, spamassassin, clamav, amavisd, authlib, sasl2 и других
* Превышение количества подключений

### misc_templates
* Всяко-разно для себя

### MySQL
* Разные проверки по мускулу

### APC NetBotz Rack Monitor 200
* Температура
* Влажность

### Nginx
* Соединения, запросы, чтение, запись, ожидание и т.п.

### not_tested
* Еще не тестировал и не разбирался

### Postfix
* Очередь
* Кол-во полученных и отправленных байт
* Кол-во олученных и отправленных писем и другие параметры

### APC Symmetra LX 16000
* Actual Battery Bus Voltage
* Battery Capacity
* Input Frequency
* Input Voltage L1
* Input Voltage L2
* Input Voltage L3
* Internal Temperature
* Nominal Battery Voltage
* Output Current
* Output Frequency
* Output Load
* Output Voltage
* Runtime Remaining
* etc

### vmware_esx
* Старый шаблон

### vmware_esxi
* Обнаружение гипервизоров и ВМ
* Мониторинг разных параметров
* Иногда не работает :( (пока не понял причину)

### Windows
* Стандартные шаблоны с добавленными элементами данных и триггерами для мониторинга текущего времени на серверах.
* Шаблон, для мониторинга резервного копирования серверов с помощью "Система архивации данных Windows Server".


#### Zabbix repository
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


#### Переименование шаблона до импорта
Открыть `xml`-файл шаблона, найти в начале файле (обычно первые 10-15 строк) строку вида `<template>Template_Name</template>` и далее сделать поиск по всему файлу по подстроке `Template_Name` и заменить на, например, `Template_Name_2`. После чего можно импортировать "переименованный" шаблон, отредактировать в интерфейсе заббикса и подключить к нужным узлам/шаблонам.


