# zabbix templates

## APC Symmetra LX

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


`zabbix_screens.symmetra_lx.xml` - файл, для импорта комплексных экранов.

`zabbix_hosts.symmetra_lx.xml` - файл, для импорта комплексных экранов, на которых отображаются данные сразу с двух симметр. Я не нашел, как такое сделать в шаблоне. Будут импортированы два хоста. Необходим для `zabbix_screens.symmetra_lx.xml`.

