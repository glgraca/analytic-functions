# Paging results

When looping through a paged resultset, a few extra columns might help make the code shorter and simpler:

1. Total number of rows;
1. First value in the results;
1. Last value in the results.

With these values it is easier to set up the paging links in a web application, for instance.

```sql
select ord, table_name, first_name, last_name, total_rows  from (
 select table_name,
        row_number() over (order by table_name asc) ord,
        count(1) over () total_rows,
        first_value(table_name) over (order by table_name asc rows between unbounded preceding and unbounded following) first_name,
        last_value(table_name) over (order by table_name asc rows between unbounded preceding and unbounded following) last_name
 from all_tables 
 where table_name like 'B%'
 order by table_name desc
) where  ord between 10 and 19
order by ord
```

This query will give you 10 rows (from 10 to 19) of table names that start with B. 

It will also inform:

1. The total number of tables with names that start with B;
1. The very first table that starts with a B (it will have row_number()=1);
1. The last table that starts with a B (it will have row_number()=total_rows).
