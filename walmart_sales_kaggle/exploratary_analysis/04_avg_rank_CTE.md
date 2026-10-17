## Introduction: 
I am here trying to use CTE (comman table expressions) to make the query more complicated and more steps to compute. 
I want to see the difference between holiday and normal weeks, percentage, ranking of stores on the basis of overall, holiday and non holiday weeks. 
```sql
With store_avg as (SELECT 
	store as store_number,
	count (*) as total_weeks,
    COUNT(CASE WHEN Holiday_flag = FALSE THEN 1 END) AS total_normal_weeks,
    COUNT(CASE WHEN Holiday_flag = TRUE THEN 1 END) AS total_holiday_weeks,
	Round(AVG(weekly_sales)::numeric, 2) as overall_avg,
    ROUND(AVG(CASE WHEN Holiday_flag = FALSE THEN weekly_sales END)::numeric, 2) AS non_holiday_avg,
    ROUND(AVG(CASE WHEN Holiday_flag = TRUE THEN weekly_sales END)::numeric, 2) AS holiday_avg
FROM walmart_sales
group by store),
percentage_change as (select *,
round (((holiday_avg-non_holiday_avg)/nullif(non_holiday_avg, 0)*100) :: numeric, 2) as sales_difference_pct
 from store_avg),
 avg_rank as
 (select *,
Rank () over (order by overall_avg desc) as Overall_avg_rank,
Rank () over (order by non_holiday_avg desc) as non_holiday_rank,
Rank () over (order by holiday_avg desc) as holiday_rank,
Rank () over (order by sales_difference_pct) as diff_pct_rank_sales
from percentage_change)
Select 
store_number,
overall_avg, 
overall_avg_rank,
non_holiday_avg, 
non_holiday_rank,
holiday_avg, 
holiday_rank, 
sales_difference_pct, 
diff_pct_rank_sales, 
Total_weeks, 
Total_normal_weeks, 
Total_holiday_weeks 
from avg_rank
order by overall_avg desc;
```
***Results:**
