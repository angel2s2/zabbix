# zabbix templates

## APC Symmetra LX
Я использовал APC Symmetra LX 16000. Возможно на других Symmetra LX тоже будет работать. Проверить негде.


### Мониторинг параметров:
+ Actual Battery Bus Voltage
+ Battery Capacity
+ Input Frequency
+ Input Voltage L1
+ Input Voltage L2
+ Input Voltage L3
+ Internal Temperature
+ Nominal Battery Voltage
+ Output Current
+ Output Frequency
+ Output Load
+ Output Voltage
+ Runtime Remaining
+ Alarms Present
+ Seconds On Battery


`zabbix_screens.symmetra_lx.xml` - комплексные экраны

### Настройка SNMP
В веб-интерфейсе симметры перейти в Administration -> Network -> SNMPv1 (шаблон для 1й версии сделан). В `access` поставить галку и нажать "Apply". В `access control` тыкнуть любую строку, там в "Community Name" указать `zabbix`, в "NMS IP/Host Name" - IP заббикс сервера, в "Access Type"  выбрать `Read`. Нажать "Apply".


### Мониторинг через Syslog
Выполнить настройки описанные в [syslog](https://github.com/angel2s2/zabbix/tree/master/syslog).
Настроить пересылку логов на symmetra lx:
* В `Logs -> Syslog -> servers -> Add Server` указать адрес rsyslog-сервера, порт, язык и протокол.
* В `Logs -> Syslog -> settings` поставить галку `Message Generation`, в `Facility Code` выбрать источник, например, local4.

В шаблон `Template Syslog` добавить нужные триггеры на основе функций `iregxp()`, `regxp()`, `str()`. 

