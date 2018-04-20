# Count all rows

Sometimes you want to know how many rows you will read before you've looped through all of them. 

An extra column with the total row count will be quite handy.

```sql
select table_name, count(1) over ()
from all_tables
```
