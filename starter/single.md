# Single instance

Развертывание возможно через стартер и вручную. вручную как на ВМ или сервере, так и в докере.

- [ArangoDB Starter](#arangodb-starter)

- [ArangoDB Starter в докере](#arangodb-starter-docker)

- [Ручной запуск](#manual-start)

- [Ручной запуск в докере](#manual-start-docker)

---

## ArangoDB Starter

Для начала рекомендуется сгенерировать секретку:

```bash 
$ arangodb create jwt-secret --secret=arangodb.secret
$ chmod 400 arangodb.secret
```

Запуск автономного единичного экземпляра. В параметре auth.jwt-secret нужно указать путь к ранее созданной секретке:

```bash
arangodb --starter.mode=single --auth.jwt-secret=arangodb.secret
```
 
 для остановки ввести команду

 ```bash
 arangodb stop
 ```

## ArangoDB Starter. Docker

Для запуска автономного экземпляра в докере выполнить:

```bash
$ export IP=<IP of docker host>
$ docker volume create arangodb
$ docker run -it --name=adb --rm -p 8528:8528 \
    -v arangodb:/data \
    -v /var/run/docker.sock:/var/run/docker.sock \
    arangodb/arangodb-starter \
    --starter.address=$IP \
    --starter.mode=single 
```

---


## Manual start

Предполагается что 127.0.0.1 и порт 8529 свободны. Для запуска нужно создать каталог с бд, например /var/lib/arangodb3.standalone.

```bash
arangod --server.endpoint tcp://0.0.0.0:8529 --database.directory /var/lib/arangodb3.standalone &
```

## Manual Start. Docker

Запуск в докере:

```bash
$ docker run -e ARANGO_NO_AUTH=1 -p {local_ip}:10000:8529 arangodb/arangodb arangod \
  --server.endpoint tcp://0.0.0.0:8529\
  ```

Для докера обязательно указывать метод аутентификации, иначе не запустится.

- ARANGO_NO_AUTH=1.
     Полностью отключена аутентификация. Полезно для локального тестирования или для работы в доверенной сети (без общедоступного интерфейса).

- ARANGO_ROOT_PASSWORD={password}. 
    Запуск ArangoDB с указанным паролем для root.

- ARANGO_RANDOM_ROOT_PASSWORD=1. 
    ArangoDB сгенерирует случайный пароль root
