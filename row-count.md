# Count all rows

Sometimes you want to know how many rows you will read before you've looped through all of them. 

An extra column with the total row count will be quite handy.

```sql
select table_name, count(1) over ()
from all_tables
```
If the query uses a group by, you'll need to sum the different counts:

```sql
select count(1), status, sum(count(1)) over()
from all_tables
group by status
```

This query you give you three columns:

1. The count for a given status;
1. The status;
1. The overall count (the sum of all tables in all status).

The third column can't use a simple "count(1) over()" because that would only give you the total number of rows in your resultset (i.e. the number of distinct status).
