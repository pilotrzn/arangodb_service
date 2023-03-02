##  для создания графа
в коллекцию типа Edge записываются ключи _from и _to, в ключах ссылки на id объектов других коллекций


## для получения данных от детей к родителям

получим id конкретного ребенка

```sql
FOR c IN Characters
  FILTER c.name == "Bran"
  RETURN c._id
  ```

  далее пройдемся по коллекции edge, получим всех родителей указанного ребенка. Для этого импользуется OUTBOUND
  
  ```sql
FOR c IN Characters
    FILTER c.name == "Bran"
FOR v IN 1..1 OUTBOUND c ChildOf
    RETURN v.name
 ```

## данне от родителей к детям
  ```sql
FOR c IN Characters
    FILTER c.name == "Ned"
FOR v IN 1..1 INBOUND c ChildOf
    RETURN v.name
```

если нужно больше данных, например внуков, сделать больше шагов

  ```sql
FOR c IN Characters
    FILTER c.name == "Tywin"
FOR v IN 2..2 INBOUND c ChildOf
    RETURN v.name
```

в данном случае вернется 2 раза одинаковое значение.
чтобы исправить  применяется параметр обхода:

  ```sql
FOR c IN Characters
    FILTER c.name == "Tywin"
  FOR v IN 2..2 INBOUND c ChildOf OPTIONS { uniqueVertices: "global", order: "bfs" }
    RETURN v.name
```

```
uniqueVertices: "global"
```

Настройка подавляет повторяющиеся вершины на ранней стадии


## от детей к родителям и дедам-бабушкам

  ```sql
FOR c IN Characters
    FILTER c.name == "Joffrey"
  FOR v IN 1..2 OUTBOUND c ChildOf OPTIONS { uniqueVertices: "global", order: "bfs" }
    RETURN DISTINCT v.name
```
