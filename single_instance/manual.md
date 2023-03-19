## Manual start

Предполагается что 127.0.0.1 и порт 8529 свободны. Для запуска нужно создать каталог с бд, например /var/lib/arangodb3.standalone.

```bash
arangod --server.endpoint tcp://0.0.0.0:8529 /
        --database.directory /var/lib/arangodb3.standalone &
```

Для запуска в фоне в конце команды ставим &

[Назад](./README.md)
