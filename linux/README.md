# zabbix templates

## Template OS Linux
Стандартный шаблон для мониторинга ОС GNU/Linux.

### Изменения
 + элемент данных `system.localtime` (локальное время на клиенте)
 + триггер `system.localtime.fuzzytime(180)` (тревога при рассинхронизации клиента и сервера на 3+ минут)

