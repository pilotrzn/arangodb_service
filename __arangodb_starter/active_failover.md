# Развертывание отказоустойчивой системы (Активный переход)

Перед запуском необходимо создать секретку и раскопировать ее на каждый сервер

```bash
# arangodb create jwt-secret --secret=arangodb.secret
# chmod 400 arangodb.secret
```

Пример запуска на одной машине. Будут созданы отдельные каталоги и порты для каждого  экземпляра: 

```bash
arangodb --starter.local --starter.mode=activefailover --starter.data-dir=./localdata --auth.jwt-secret=/etc/arangodb.secret --agents.agency.supervision-grace-period=30
```

Вариант с 3 ВМ. Команду последовательно выполнить на каждой ВМ:

arangodb --starter.mode=activefailover --starter.data-dir=/arangodb/starter/ --auth.jwt-secret=/arangodb/arangodb.secret --agents.agency.supervision-grace-period=30 --starter.join alt9-2-srv01,alt9-2-srv02,alt9-2-srv03


[Назад](./README.md)