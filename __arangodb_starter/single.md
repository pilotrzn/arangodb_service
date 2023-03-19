# Single instance

Развертывание через стартер на ВМ, на сервере, или в докере.

- [ArangoDB Starter](#arangodb-starter)

- [ArangoDB Starter в докере](#arangodb-starter-docker)

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

[Назад](./README.md)

