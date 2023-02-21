запрос к 2 коллекциям с фильтром:

```sql
FOR c IN Characters
  FOR key IN c.traits
    FOR t IN Traits
      FILTER t._key == key
      RETURN t
```

