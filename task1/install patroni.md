## Схема установки
![Схема установки](https://github.com/vloldik/devopspractice/blob/main/task1/scheme.png?raw=true)
- Т.к. при настройке локальных машин была выставлена память в следующей конфигурации:
  1. 192.168.0.123 - 2048 мб.
  2. 192.168.0.124 - 1024 мб.
  3. 192.168.0.125 - 2048 мб.

Прокси сервер будем ставить на машину `ii`, предварительно удалив с нее postgres за ненадобностью ?*процесс удаления не включен*
Довольно обычным решением будет настроить load balancer следующим образом:
  - `master` работает на запись
  - `slave` работает на чтение
В большей рахитектуре можно было бы раскидать нагрузку круглым робином между slave нодами, но в данной ситуации думаю этого будет достаточно

## Установка (машины `192.168.0.123` `192.168.0.125`)
**\[ВАЖНО\]** нужно переустановить python, в случае, если он был установлен по старой инструкции
 см. [установка python](https://github.com/vloldik/devopspractice/blob/main/task1/install%20python.md)

```
python3.11 -m pip install psycopg2-binary
python3.11 -m pip install patroni[etcd3,psycopg2-binary]
```
создадим config.yaml для patroni
```bash
mkdir /etc/patroni
vi /etc/patroni/config.yaml
```
Вставим содержимое [файла](https://github.com/vloldik/devopspractice/blob/main/task1/external/config.yaml), заменяя поле `name`, и ip следующим образом:
 - `listen` и `connect_address` должны быть открыты для общения с машинами в сети
 - ip пользователя replicator должен соответствовать ip машин, на которых запущен patroni
 - пароли обязательно сменить
- Открываем порты:
```bash
sudo firewall-cmd --permanent --add-port=8008/tcp --add-port=5432/tcp
sudo firewall-cmd --reload
```
- Добавим удобный алиас на будущее (необязательно)
```
alias clusterctl='patronictl -c /etc/patroni/config.yml'
```
- В папку /etc/systemd/system/ загрузим [patroni.service](https://github.com/vloldik/devopspractice/blob/main/task1/external/patroni.service)
- Включим и запустим сервис
```
systemctl daemon-reload
systemctl enable patroni
systemctl start patroni
```

команда `clusterctl list` должна вывести список из двух серверов
