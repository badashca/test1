# Решение задач
## Задача 0

* выбрать все записи из 2-х таблиц уникальные
```sql
select distinct Group, SOF, Requests
from 
table1
union all 
table2
```

* не уникальные
```sql
select Group, SOF, Requests
from 
table1
union all 
table2
```
* cтроки, участвующие в 2-х таблицах одновременно. Совпадение должно быть по всем 3-м полям.
```sql
select Group, SOF, Requests
from 
table1
inner join
table2
using (Group, SOF, Requests)
```


## Задача 1
```sql
select Group, SOF, Requests
from 
table1
inner join
table2
using (Group, SOF, Requests)
```

## Задача 2
```sql
select * from
```
