# запуск 3 экземпляров Agency

```bash
arangod --server.endpoint tcp://0.0.0.0:5001 \
  --agency.my-address=tcp://127.0.0.1:5001 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory /arangodb3/agent1 &
   
arangod --server.endpoint tcp://0.0.0.0:5002 \
  --agency.my-address=tcp://127.0.0.1:5002 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory /arangodb3/agent2 &

arangod --server.endpoint tcp://0.0.0.0:5003 \
  --agency.my-address=tcp://127.0.0.1:5003 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --agency.supervision-grace-period 30 \
  --database.directory /arangodb3/agent3 &
```


# Запуск эксземпляров через Starter

```bash
arangodb --starter.local \
--starter.mode=activefailover \
--starter.data-dir=./localdata \
--auth.jwt-secret=/arangodb3/arangodb.secret \
--agents.agency.supervision-grace-period=30 &
``
