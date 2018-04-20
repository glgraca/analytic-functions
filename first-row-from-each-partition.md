# First row from each partition

You want to partition your data and select only the first row from each one.

```sql
select * from (
  select table_name, 
    tablespace_name, 
    num_rows, row_number() over (partition by tablespace_name order by num_rows asc) ord
  from all_tables 
) where ord=1
```

Use row_number() with the desired parition and ordering. Then use an outer select to filter those rows that have row_number()=1.

This query selects the smallest (in number of rows) tables from each tablespace.

If you need the know the last row in each partition, invert the order by (and add "nulls last" if necessary).

```sql
select * from (
  select table_name, 
    tablespace_name, 
    num_rows, row_number() over (partition by tablespace_name order by num_rows desc nulls last) ord
  from all_tables 
) where ord=1
```
