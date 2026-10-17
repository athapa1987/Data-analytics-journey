Further breaking the sales record
calculated using aggregate function (`SUM`, `AVG`, `STDDEV`, `MIN`, `MAX`) and group by store and also displayed store number to further analysis
1. ordered by total sales in descending order to answer which store have highest sales to lowest sales

```sql
 SELECT 
 	store as store_number,
    COUNT(*) AS total_records,
    ROUND(SUM(weekly_sales)::numeric, 2) AS total_revenue,
    ROUND(AVG(weekly_sales)::numeric, 2) AS avg_weekly_sales,
    ROUND(MIN(weekly_sales)::numeric, 2) AS min_weekly_sales,
    ROUND(MAX(weekly_sales)::numeric, 2) AS max_weekly_sales,
    ROUND(STDDEV(weekly_sales)::numeric, 2) AS std_dev_sales
FROM walmart_sales
group by store
order by total_revenue desc;
```
**wanted to find out total number of stores**
```sql
select count (distinct store) AS unique_store_count
FROM walmart_sales;
```
*This was not need maybe as the row numbers were there.*

*I did not include the query output as it takes long time to type here.*
***Results:***
*From the above query:*
* There were total 45 stores with 143 weeks of sales. 
* Store 20 have highest sales of 301M while 33 have lowest revenue around 37M. Store numbers 4, 14, 13, 2 are second, third and so on. As the numbers of weeks are same so the average sales are also in similar order. As we don't know the size of store, number of employees and other costs so we could not comment on profitability of the stores. 
* Using `ORDER BY` standard deviation store 14 comes top of the sales while store 37 comes to the last. From the results it shows that the stores with higher sales have more variation than stores having lower sales as least variation or consistentancy in weekly sales because they have lower standard deviation. However, it also noted that the individual average revenue are more dispersed or scattered comparison to the all store average.  
* changed order by to max weekly sales at store 14 and min weekly sales at store 4 (the order shows the highest and lowest sales of the store and arranged by this functions)
4. I did not ordered by avg weekly sales as it will be the same order as total revenue
