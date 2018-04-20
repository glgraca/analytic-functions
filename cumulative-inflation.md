# Cumulative Inflation

Suppose you have a table with monthly inflation values and you need to calculate the total value for the last year.

```sql
create table inflation (
  month date,
  inflation number(5,2)
);
```

You need to multiply the last 12 values, but SQL does not offer a multiply() aggregate function. We must make do with sum().

Rememeber that exp(ln(x)+ln(y)) = x*y. 

We also have to add 1 to the monthly values and then discount it from the total (0.23% inflation turns into 1.23).

```sql
select month, inflation, (exp(accumulated)-1)*100 from (
  select month, inflation, 
    sum(ln(1+(inflation/100)))
      over (order by month desc rows between current row and 11 following)
        as accumulated
  from inflation
)
```
The accumulated column has sum(ln(1+(inflation/100))) over the current row plus the eleven previous rows (months).

Therefore, the resultset will give you three columns: month, inflation for the current month, and inflation for the last 12 months.
