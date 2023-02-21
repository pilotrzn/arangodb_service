
Запрос с фильтрацией и указанием полей

```sql
FOR c IN Characters
  FILTER c.age < 13
  FILTER c.age != null
  RETURN { name: c.name, age: c.age }
```

Фильтрации могут быть такими:
  
```sql
  FILTER c.age < 13
  FILTER c.age != null
```

и такими  

```sql
FILTER c.age < 13 AND c.age != null
```

использование OR:

```sql
FOR c IN Characters
  FILTER c.name == "Jon" OR c.name == "Joffrey"
  RETURN { name: c.name, surname: c.surname }
  ```

Использование AND и OR:

```sql
FOR c IN Characters
  FILTER c.age < 18 AND c.name == "Jon" OR c.name == "Joffrey"
  RETURN c
```

```sql
FOR c IN Characters
  FILTER (c.age < 18 AND c.name == "Jon") OR (c.name == "Joffrey")
  RETURN c
```

