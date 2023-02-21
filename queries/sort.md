Способы сортировки


```sql
FOR c IN Characters
  SORT c.name
  LIMIT 10
  RETURN c.name
```

```sql
FOR c IN Characters
  SORT c.name DESC
  LIMIT 10
  RETURN c.name
  ```

  Сортировка по нескольким атрибутам:

  ```sql
  FOR c IN Characters
  FILTER c.surname
  SORT c.surname, c.name
  LIMIT 10
  RETURN {
    surname: c.surname,
    name: c.name
  }
  ```

  ```sql
  FOR c IN Characters
  FILTER c.age
  SORT c.age
  LIMIT 10
  RETURN {
    name: c.name,
    age: c.age
  }
  ```