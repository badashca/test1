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
Существует таблица Exam_Merge, в которую импортируются записи из аналогичной по структуре таблицы в другой базе данных. При этом таблица Exam_Merge содержит значения первичного ключа ID из другой базы данных, но это поле не является в ней уникальным. По некоторым причинам часть записей в ней были дублированы: запись с одним и тем же ID содержится в таблице несколько раз, значения остальных полей дублированных записей так же совпадают.
Необходимо удалить дубли, оставив только не повторяющиеся ID


Самый простой вариант, для внешнего использования изложен ниже:
```sql
SELECT distinct * FROM Exam_Merge ORDER BY ID
```
Но, если по каким-то причинам неободимо хранить уникализированную таблицу с возможностью realtime insert, то необходимо во время процедуры удаления дублей, блокировать транзакции на запись и чтение к таблице, чтобы случайно не проронить записи, которые в нее добавляются.

Процедура удаления дублей:
```sql
SELECT distinct * 
into #tmp_table
FROM Exam_Merge;

truncate table Exam_Merge;

select *
into Exam_Merge
from #tmp_table;

commit;
```
