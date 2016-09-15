# zabbix templates

## APC NetBotz Rack Monitor 200

### Мониторинг параметров
Мониторит Humidity & Temperature. Шаблон использует низкоуровневое обнаружение (LLD).


Основано на https://share.zabbix.com/monitoring-equipment/apc-netbotz-rack-monitor-200
Отличие в том, что в шаблоне по ссылке есть "баг" - в качестве значения для (любого) сенсора используются данные от первого сенсора. В моем шаблоне это исправленно.

PS: Тестимровал только на версии zabbix - 3.2.0.

