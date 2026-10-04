Further breaking the sales record
calculated using aggregate function (`SUM`, `AVG`, `STDDEV`, `MIN`, `MAX`) and group by store and also displayed store number to further analysis
1. ordered by total revenue in descending order to answer which store have higher sales and lower sales

'''sql
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
'''
###
store 20 have highest sales while 33 have lowest revenue.

2. Using same codes or commands but changing ordered by to standard deviation understand which store sales are unpredictable, store 14 have highest std it means sales are highly flucted there
3. changed order by to max weekly sales at store 14 and min weekly sales at store 4 (the order shows the highest and lowest sales of the store and arranged by this functions)
4. I did not ordered by avg weekly sales as it will be the same order as total revenue
