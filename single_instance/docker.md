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

---

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

[Назад](./README.md)