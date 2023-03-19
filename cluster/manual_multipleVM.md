# Настройка на 3 ВМ

Настройка на нескольких ВМ отличается тем, что нужно заменить все локальные адреса 127.0.0.1 на фактические адреса серверов.

в Нашем распоряжении 3 машины:

- 192.168.1.1
- 192.168.1.2
- 192.168.1.3

Предполагается что на каждой из машин работает 1 агент, 1 координатор и 1 БД. Для такой конфигурации будут задействованы порты:

- 8531 как порт агентов
- 8530 как порт серверов БД
- 8529 как порт координаторов.

## Agency

- 192.168.1.1

```bash
$ sudo arangod --server.endpoint tcp://0.0.0.0:8531 \
  --agency.my-address tcp://192.168.1.1:8531 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.supervision true \
  --database.directory agent 
```

- 192.168.1.2

```bash
$ sudo arangod --server.endpoint tcp://0.0.0.0:8531 \
  --agency.my-address tcp://192.168.1.2:8531 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.supervision true \
  --database.directory agent
```

- 192.168.1.3

```bash
$ sudo arangod --server.endpoint tcp://0.0.0.0:8531 \
  --agency.my-address tcp://192.168.1.3:8531 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://192.168.1.1:8531 \
  --agency.endpoint tcp://192.168.1.2:8531 \ 
  --agency.endpoint tcp://192.168.1.3:8531 \
  --agency.supervision true \
  --database.directory agent
```

## DB-servers

- 192.168.1.1

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8530 \
  --cluster.my-address tcp://192.168.1.1:8530 \
  --cluster.my-role DBSERVER \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory dbserver &
```

- 192.168.1.2

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8530 \
  --cluster.my-address tcp://192.168.1.2:8530 \
  --cluster.my-role DBSERVER \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory dbserver &
```

- 192.168.1.3

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8530 \
  --cluster.my-address tcp://192.168.1.3:8530 \
  --cluster.my-role DBSERVER \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory dbserver &
```

## Coordinators

- 192.168.1.1

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8529 \
  --cluster.my-address tcp://192.168.1.1:8529 \
  --cluster.my-role COORDINATOR \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory coordinator &
```

- 192.168.1.2

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8529 \
  --cluster.my-address tcp://192.168.1.2:8529 \
  --cluster.my-role COORDINATOR \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory coordinator &
```

- 192.168.1.3

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8529 \
  --cluster.my-address tcp://192.168.1.3:8529 \
  --cluster.my-role COORDINATOR \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory coordinator &
```

Если есть необходимость, можно расширить количество БД и ккоординаторов. Например на ВМ с адресом 192.168.1.4:

```bash
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8530 \
  --cluster.my-address tcp://192.168.4.1:8530 \
  --cluster.my-role DBSERVER \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory dbserver &
  
$ sudo arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:8529 \
  --cluster.my-address tcp://192.168.1.4:8529 \
  --cluster.my-role COORDINATOR \
  --cluster.agency-endpoint tcp://192.168.1.1:8531 \
  --cluster.agency-endpoint tcp://192.168.1.2:8531 \
  --cluster.agency-endpoint tcp://192.168.1.3:8531 \
  --database.directory coordinator &
```
