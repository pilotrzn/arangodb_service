Запрос с ограничением:

```sql
FOR c IN Characters
  LIMIT 5
  RETURN c.name
```

Запрос, прокпускающий заданное количество строк, и возвращающий указанное:

```sql
FOR c IN Characters
  LIMIT 2, 5
  RETURN c.name
```

