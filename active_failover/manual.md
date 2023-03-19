# Развертывание отказоустойчивой системы (Активный переход) в ручном режиме

Для работы в таком режиме требуется наличие Лидера(Leader), последователя(Follower) и хотя бы одного сервера Агента(Agency).
Агент выступает в роли свидетеля (witness)

Используется асинхронный режим репликации(с использованием WAL).

## Вариант с 3 агентами и 2 нодами БД, пример на 1 ВМ, используются разные порты.

Сначала запускаются 3 экземпляра Agency:

```bash
arangod --server.endpoint tcp://0.0.0.0:5001 \
  --agency.my-address=tcp://127.0.0.1:5001 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory agent1 &
   
arangod --server.endpoint tcp://0.0.0.0:5002 \
  --agency.my-address=tcp://127.0.0.1:5002 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory agent2 &

arangod --server.endpoint tcp://0.0.0.0:5003 \
  --agency.my-address=tcp://127.0.0.1:5003 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory agent3 &
```

Все экземпляры создадутся в текущем каталоге, для каждого будет создан каталог, указанный в  --database.directory.

Стоит обратить внимание, что во избежание ненужных сбоев может иметь смысл увеличить значение параметра запуска --agency.supervision-grace-period до значения, превышающего 30 секунд.

Далее запускаются 2 автономных экземпляра:

```bash
arangod --server.authentication false \
  --server.endpoint tcp://127.0.0.1:6001 \
  --cluster.my-address tcp://127.0.0.1:6001 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \ 
  --replication.automatic-failover true \
  --database.directory singleserver6001 &
 
arangod --server.authentication false \
  --server.endpoint tcp://127.0.0.1:6002 \
  --cluster.my-address tcp://127.0.0.1:6002 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \
  --replication.automatic-failover true \
  --database.directory singleserver6002 &
```

## Вариант с несколькими машинами(ВМ)

В примере испольуются 3 ВМ с адресами:

- 192.168.122.84
- 192.168.122.107
- 192.168.122.168

### Инстансы Agency 

- ВМ 192.168.122.84

```bash
arangod --server.endpoint tcp://0.0.0.0:8531 \
  --agency.my-address tcp://192.168.122.84:8531 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory /arangodb/agent
```

- ВМ 192.168.122.107

```bash
arangod --server.endpoint tcp://0.0.0.0:8531 \
  --agency.my-address tcp://192.168.122.107:8531 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory /arangodb/agent
```

- ВМ 192.168.122.168

```bash
arangod --server.endpoint tcp://0.0.0.0:8531 \
  --agency.my-address tcp://192.168.122.168:8531 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://192.168.122.84:8531 \
  --agency.endpoint tcp://192.168.122.107:8531 \
  --agency.endpoint tcp://192.168.122.168:8531 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory /arangodb/agent
```

### Инстансы БД

- ВМ 192.168.122.84

```bash
arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8529 \
  --cluster.my-address tcp://192.168.122.84:8529 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://192.168.122.84:8531 \
  --cluster.agency-endpoint tcp://192.168.122.107:8531 \
  --cluster.agency-endpoint tcp://192.168.122.168:8531 \
  --replication.automatic-failover true \
  --database.directory /arangodb/singleserver &
```

- ВМ 192.168.122.107

```bash
arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8529 \
  --cluster.my-address tcp://192.168.122.107:8529 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://192.168.122.84:8531 \
  --cluster.agency-endpoint tcp://192.168.122.107:8531 \
  --cluster.agency-endpoint tcp://192.168.122.168:8531 \
  --replication.automatic-failover true \
  --database.directory /arangodb/singleserver &
```

[Назад](./README.md)
