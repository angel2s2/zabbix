# zabbix templates

## Template OS Windows
Стандартный шаблон для мониторинга ОС Windows.

### Изменения
 + элемент данных `system.localtime` (локальное время на клиенте)
 + триггер `system.localtime.fuzzytime(180)` (тревога при рассинхронизации клиента и сервера на 3+ минут)

