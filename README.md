# Coffee-Sales-SQL
A project to gather business insights for coffee shops based on data gathered in New York over the first six months of 2023.

## Dataset
Maven Analytics Coffee Shop Sales dataset. URL: https://www.mavenanalytics.io/data-playground

## Query 1

### What is the trend for Coffee Sales Revenue?

### SQL Query
We find the total revenue by month using (quantity × unit price).

```sql
SELECT
    DATE_TRUNC('month', transaction_date) AS month,
    ROUND(SUM(transaction_qty * unit_price), 2) AS total_sales
FROM coffee_sales
GROUP BY DATE_TRUNC('month', transaction_date)
ORDER BY month;
```
### Output
```
         month          | total_sales 
------------------------+-------------
 2023-01-01 00:00:00+00 |    81677.74
 2023-02-01 00:00:00+00 |    76145.19
 2023-03-01 00:00:00+00 |    98834.68
 2023-04-01 00:00:00+01 |   118941.08
 2023-05-01 00:00:00+01 |   156727.76
 2023-06-01 00:00:00+01 |   166485.88
(6 rows)
```
### Insight
Revenue generally trends upwards with significant differences of around double in some months from Q1 to Q2. As a result there is a significant increase in demand for coffee in Q2 than in Q1 and further investigation is needed to determine what is driving this relationship.

## Query 2

### What are the best selling products by revenue?

### SQL Query


### Output


### Insight
