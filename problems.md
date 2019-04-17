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
select a.Group as Group, a.SOF as SOF, a.Requests as Requests
from 
table1 as a
inner join 
table2 as b 
on a.Group=b.Group 
and a.SOF=b.SOF 
and a.Requests=b.Requests

```

* вернуть все значения из 1 таблицы, которых нет во второй.
```sql
select a.Group as Group, a.SOF as SOF, a.Requests as Requests
from table1 as a
left join table2 as b 
on a.Group=b.Group 
and a.SOF=b.SOF 
and a.Requests=b.Requests
where b.Group is null
```

* вернуть все записи и из таблицы 1, и из таблицы 2 таким образом, чтобы каждая строка таблицы 1 привязывалась к каждой строке таблицы 2..
```sql
select *
from table1 ,table2 
```
* из таблицы 1 вернуть записи, у которых Requests больше 5
```sql
select *
from table1
where Requests>5
```
* выбрать уникальные группы (Group) из таблицы 2
```sql
select distinct Group
from table2
```
* выбрать все строки из 1 и 2 таблицы, у которых совпадает кол-во заявок (Requests). Рез-т должен отражать поля Group и SOF из 2-х таблиц, а также одну колонку с Requests.
```sql
select a.Group as Group_a, a.SOF as SOF_a, 
b.Group as Group_b, b.SOF as SOF_b,
a.Requests as Requests
from 
table1 as a
inner join 
table2 as b 
on a.Requests=b.Requests
```
* Отобрать те SOF из таблицы 1, по которым нет заявок в таблице 2
```sql
select a.SOF as SOF
from 
table1 as a
left join 
table2 as b 
on a.SOF=b.SOF
where b.SOF is null

```
## Задача 1
Выбрать ФИО клиентов, заключивших более одного кредитного договора в 2010 году, так же номера и даты выдачи этих договоров.
Упорядочить по ФИО клиента и дате выдачи договора.

```sql
with a as
(
    select
        c.ClID as ClID,
        c.ClFullName as [ФИО Клиента],
        count (d.DlID ) as cnt
    from Clients as c
    left join Deals as d 
    on c.ClID = d.DlClient 
    group by 
        c.ClID ,
        c.ClFullName
    having count (d.DlID ) > 1
)
select 
    a.[ФИО Клиента],
    d.DlCode as [Номер договора],
    d.DlValutationDate as [Дата выдачи]
from a
left join Deals as d 
on a.ClID = d.DlClient 
```

## Задача 2
```sql
select * from
```
