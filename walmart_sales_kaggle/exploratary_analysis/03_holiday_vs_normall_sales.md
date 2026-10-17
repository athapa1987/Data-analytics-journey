***Comparing holiday vs normal week sales***
**Problem:** 
As the number of holiday weeks and normal weeks are not same. Therefore, I am using average or mean of normal and holiday week to make comparision. 
```sql
SELECT 
	store as store_number,
    COUNT(CASE WHEN Holiday_flag = FALSE THEN 1 END) AS total_normal_weeks,
    COUNT(CASE WHEN Holiday_flag = TRUE THEN 1 END) AS total_holiday_weeks,
    ROUND(AVG(CASE WHEN Holiday_flag = FALSE THEN weekly_sales END)::numeric, 2) AS overall_normal_avg,
    ROUND(AVG(CASE WHEN Holiday_flag = TRUE THEN weekly_sales END)::numeric, 2) AS overall_holiday_avg
FROM walmart_sales
group by store
order by overall_holiday_avg;
```

***Findings:***
* There were 133 normal and 10 holiday weeks.
* In general, weekly average sales is higher on holiday weeks than normal weeks.
* store 20 is top store holiday sales and 33 is lowest holiday average sales. 
