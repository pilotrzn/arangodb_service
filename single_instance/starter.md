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

[Назад](./README.md)