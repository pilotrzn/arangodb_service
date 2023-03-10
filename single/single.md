Запуск бд через стартер

```bash
arangodb --starter.mode=single --auth.jwt-secret=arangodb.secret --starter.data-dir=/var/lib/arangodb3.start &
```
 
 для остановки ввести команду

 ```bash
 arangodb stop
 ```



Вручную

```bash
arangod --server.endpoint tcp://0.0.0.0:8529 --database.directory /var/lib/arangodb3.standalone &
```