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


docker run -e ARANGO_NO_AUTH=1 -p 10000:8529 arangodb/arangodb arangod \
  --server.endpoint tcp://0.0.0.0:8529\
  --cluster.my-address tcp://192.168.1.13:10000 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://192.168.1.1:9001 \
  --cluster.agency-endpoint tcp://192.168.1.2:9001 \
  --cluster.agency-endpoint tcp://192.168.1.3:9001 \
  --replication.automatic-failover true 