### Просмотр текущей конфигурации БД. выполнить команду из консоли arangosh:

```sql
arangosh> db._executeTransaction({ collections: {}, action: function() {return require("internal").options(); } })
```

## Параметры и команды

### Запуск в режиме аварийной консоли:

```bash
$ arangod --console
```

### Запуск в режиме демона, с указанием pid

```bash
$  arangod --daemon --pid-file /var/run/arangodb3/ar.pid
```

### Сброс(выгрузка) доступных параметров запуска

```bash
$ arangod --dump-options > dump_opt.txt
```




arangod --database.directory = "/data/arangodb"

  arangod --server.endpoint="tcp://0.0.0.0:8529"
  
    
