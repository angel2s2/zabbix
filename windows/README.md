# zabbix templates

## zabbix_template.windows.xml
Стандартный шаблон для мониторинга ОС Windows.

### Изменения
 + элемент данных `system.localtime` (локальное время на клиенте)
 + триггер `system.localtime.fuzzytime(180)` (тревога при рассинхронизации клиента и сервера на 3+ минут)

## zabbix_template.WindowsBackup.xml
Шаблон, для мониторинга резервного копирования серверов с помощью "Система архивации данных Windows Server".

###### Тестировал только на 2012 R2. Но должно работать и на 2008, 2008 R2, 2012.

### Установка
Импортировать шаблон `templates\zabbix_template.WindowsBackup.xml` и добавить его к нужным узлам сети. На этих узлах сети нужно импортировать задачи в "Планировщик заданий" из каталога `win_task_import`:

        ZabbixAgent_BackupAttention.xml
        ZabbixAgent_BackupCompleted.xml
        ZabbixAgent_BackupStarted.xml

**!!! Важно !!!**
Перед импортом задач нужно отредактировать файлы. В аттрибутах `Arguments` нужно правильно указать пути к `zabbix_sender` и `zabbix_agentd.conf`. Так же это можно сделать при импорте задачи в окне "Создание задачи" на вкладке "Действия".

**!!! Важно !!!**
В конфиге Zabbix Anget'а обязательно должен быть указан ServerActive и HostName (как в "Имя узла сети").



src https://www.zabbix.org/wiki/Monitoring_Windows_2008,_2008R2,_2012_Server_backups

