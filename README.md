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
We find the total revenue grouping by the product category to find the top 10 highest grossing products.

```sql
SELECT
    product_type,
    ROUND(SUM(transaction_qty * unit_price), 2) AS total_revenue
FROM coffee_sales
GROUP BY product_type
ORDER BY total_revenue DESC
LIMIT 10;
```

### Output
```
     product_type      | total_revenue 
-----------------------+---------------
 Barista Espresso      |      91406.20
 Brewed Chai tea       |      77081.95
 Hot chocolate         |      72416.00
 Gourmet brewed coffee |      70034.60
 Brewed Black tea      |      47932.00
 Brewed herbal tea     |      47539.50
 Premium brewed coffee |      38781.15
 Organic brewed coffee |      37746.50
 Scone                 |      36866.12
 Drip coffee           |      31984.00
(10 rows)
```
### Insight
The Barista Espresso is by about $14,000 the biggest earner in terms of revenue. Scones are the only non beverage item that make it into the top 10 quite comfortably. From #4 to #5 there is a sharp drop in revenue of around $22,100 which means that 4 products are the main drivers of revenue accounting for 56% of revenue of the top 10 products.

## Query 3

### What is the sales growth from month to month?

### SQL Query
Generate monthly_sales essentially Query 1 and use it to find percentage change from month to month.

```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', transaction_date) AS month,
        SUM(transaction_qty * unit_price) AS total_sales
    FROM coffee_sales
    GROUP BY DATE_TRUNC('month', transaction_date)
)
SELECT
    month,
    total_sales,
    ROUND(
        (total_sales - LAG(total_sales) OVER (ORDER BY month))
        / LAG(total_sales) OVER (ORDER BY month) * 100, 2
    ) AS mom_growth_pct
FROM monthly_sales
ORDER BY month;
```

### Output
```
         month          | total_sales | mom_growth_pct 
------------------------+-------------+----------------
 2023-01-01 00:00:00+00 |    81677.74 |               
 2023-02-01 00:00:00+00 |    76145.19 |          -6.77
 2023-03-01 00:00:00+00 |    98834.68 |          29.80
 2023-04-01 00:00:00+01 |   118941.08 |          20.34
 2023-05-01 00:00:00+01 |   156727.76 |          31.77
 2023-06-01 00:00:00+01 |   166485.88 |           6.23
(6 rows)

```
### Insight
There is consistent growth in revenue from February to May, particularly after the rebound in revenue contraction between January and February, with growth slowdown into June.