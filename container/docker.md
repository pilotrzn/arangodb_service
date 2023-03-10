Вариант запуска бд в докере:

```bash
docker run \
-e ARANGO_ROOT_PASSWORD=123456 \
-p 10.0.2.15:8529:8529 -d \
--name arango-instance2 \
-v /mnt/arango:/var/lib/arangodb3 \
    arangodb/arangodb arangod \
--server.endpoint tcp://0.0.0.0:8529
```