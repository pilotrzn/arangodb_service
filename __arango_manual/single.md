## Manual start

- [Ручной запуск](#manual-start)

- [Ручной запуск в докере](#manual-start-docker)


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


[Назад](./README.md)