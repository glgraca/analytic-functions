# First and Last Values

You might need to know the first and last values in a resulset before you have read all the rows.

```sql
select table_name, 
  count(1) over (),
  first_value(table_name) over (order by table_name asc rows between unbounded preceding and unbounded following), 
  last_value(table_name) over (order by table_name asc rows between unbounded preceding and unbounded following)
from all_tables;
```
