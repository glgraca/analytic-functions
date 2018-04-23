# Ratio to report

A column has a value and you need to know what percentage it represents of the sum of all values in that particular column (and partition).

```sql 
select count(1), tablespace_name, ratio_to_report(count(1)) over ()
from all_tables
group by tablespace_name
order by 1 desc;
```

This query will give you the total number of tables in each tablespace and also the percentage of that number over the total number of tables.

ratio_to_report() is available in Oracle only. Postgresql does not support it.
