# Настройка на 1 ВМ

## Agency 

Чтобы запустить Agency, сначала его необходимо активировать. Это делается путем предоставления опции 

```
--agency.activate true.
```

Чтобы запустить Agency в его отказоустойчивом режиме, установите значение 

```
--agency.size  3
```

Затем нужно будет запустить как минимум 3 агента, прежде чем агентство начнет работу.

Во время инициализации агенты должны найти друг друга. Для этого предоставьте хотя бы один общий 

```
--agency.endpoint
```

 Затем агенты будут сами координировать запуск. Они объявят о себе своим внешним адресом, который может быть указан с помощью
 
 ```
 --agency.my-address
 ```
 
Это требуется в настройках docker с мостом или в NATed средах.

### Пример запуска:

```bash
$ arangod --server.endpoint tcp://0.0.0.0:5001 \
  --agency.my-address=tcp://127.0.0.1:5001 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --database.directory agent1 &
   
$ arangod --server.endpoint tcp://0.0.0.0:5002 \
  --agency.my-address=tcp://127.0.0.1:5002 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --database.directory agent2 &

$ arangod --server.endpoint tcp://0.0.0.0:5003 \
  --agency.my-address=tcp://127.0.0.1:5003 \
  --server.authentication false \
  --agency.activate true \
  --agency.size 3 \
  --agency.endpoint tcp://127.0.0.1:5001 \
  --agency.supervision true \
  --database.directory agent3 &
```

## Coordinators, DB-servers

Эти две роли имеют общий набор соответствующих параметров. Сначала вы должны указать роль, используя 

```
--cluster.my-role
```

Это может быть либо PRIMARY(DB-сервер), либо COORDINATOR. Обратите внимание, что DBSERVER также разрешено в качестве псевдонима для PRIMARY. Кроме того нужно указать внешнюю конечную точку (IP и порт) процесса через 

```
--cluster.my-address
```

### Пример запуска:

- DB-servers

```bash
$ arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:6001 \
  --cluster.my-address tcp://127.0.0.1:6001 \
  --cluster.my-role DBSERVER \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \
  --database.directory dbserver1 &

$ arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:6002 \
  --cluster.my-address tcp://127.0.0.1:6002 \
  --cluster.my-role DBSERVER \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \
  --database.directory dbserver2 &
```

- Coordinators

```bash
$ arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:7001 \
  --cluster.my-address tcp://127.0.0.1:7001 \
  --cluster.my-role COORDINATOR \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \
  --database.directory coordinator1 &

$ arangod --server.authentication=false \
  --server.endpoint tcp://0.0.0.0:7002 \
  --cluster.my-address tcp://127.0.0.1:7002 \
  --cluster.my-role COORDINATOR \
  --cluster.agency-endpoint tcp://127.0.0.1:5001 \
  --cluster.agency-endpoint tcp://127.0.0.1:5002 \
  --cluster.agency-endpoint tcp://127.0.0.1:5003 \
  --database.directory coordinator2 &
```

Обратите особое внимание, что адреса, приведенные в разделах

```
--cluster.my-address 
--cluster.agency-endpoint
```

 не должны использовать IP-адрес 0.0.0.0, поскольку они должны содержать фактический адрес, который может быть перенаправлен на соответствующий сервер. 
 
 ```
 --server.endpoint tcp://0.0.0.0
 ```

 просто означает, что сервер привязывается ко всем доступным сетевым устройствам со всеми доступными IP-адресами.

После регистрации в агентстве во время запуска Кластер назначит идентификатор каждому серверу. Сгенерированный идентификатор будет распечатан в журнале или может быть доступен через HTTP API путем вызова 

```
http://server-address/_admin/server/id.
```

Теперь вы запустили кластер ArangoDB и можете связаться с его координаторами (и их соответствующим веб-интерфейсом) по адресам:

```
tcp://127.0.0.1:7001
tcp://127.0.0.1:7002
```