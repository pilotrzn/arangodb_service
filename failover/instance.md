# Запуск инстансов бд

```bash
arangod --server.authentication false \
  --server.endpoint tcp://0.0.0.0:6001 \
  --cluster.my-address tcp://0.0.0.0:6001 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \ 
  --replication.automatic-failover true \
  --database.directory /arangodb3/singleserver6001 &
 
arangod --server.authentication false \
  --server.endpoint tcp://0.0.0.0:6002 \
  --cluster.my-address tcp://0.0.0.0:6002 \
  --cluster.my-role SINGLE \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \
  --replication.automatic-failover true \
  --database.directory /arangodb3/singleserver6002 &
```