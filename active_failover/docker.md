
Команда запуска одного экземпляра БД.

```bash
$ docker run -e ARANGO_NO_AUTH=1 -p 192.168.1.1:10000:8529 arangodb/arangodb arangod \
  --server.endpoint tcp://0.0.0.0:8529\
  --cluster.my-address tcp://192.168.1.1:10000 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://192.168.1.1:9001 \
  --cluster.agency-endpoint tcp://192.168.1.2:9001 \
  --cluster.agency-endpoint tcp://192.168.1.3:9001 \
  --replication.automatic-failover true 
```



[Назад](./README.md)