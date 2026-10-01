Walmart sales data analysis
This module covers aggregate calculations (`SUM`, `AVG`, `STDDEV`, `MIN`, `MAX`) with rounding to 2 decimal places performed on the Walmart sales dataset in PostgreSQL.
1. Overall Sales Summary
Calculates key summary statistics across all stores and weeks.
It only show the number of record, total revenue, average weekly sale of overall of stores, minimum and maximum weekly sales. Still it does not give any comparison and only mere vision of data what is the total sales, mean or average sales, max and min sales. Standard deviation is smaller than mean and above 50% so the sales looks volatile. Also, minimum sales and maximum sales have huge difference it is may be because of the stores size and location. With this data it is not possible for to see the location and size but I gonna look further on to data on other files to produce sensible and more insights. 
```sql
SELECT 
    COUNT(*) AS total_records,
    ROUND(SUM(weekly_sales)::numeric, 2) AS total_revenue,
    ROUND(AVG(weekly_sales)::numeric, 2) AS avg_weekly_sales,
    ROUND(MIN(weekly_sales)::numeric, 2) AS min_weekly_sales,
    ROUND(MAX(weekly_sales)::numeric, 2) AS max_weekly_sales,
    ROUND(STDDEV(weekly_sales)::numeric, 2) AS std_dev_sales
FROM walmart_sales;
