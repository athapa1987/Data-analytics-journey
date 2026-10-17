## Introduction:
As standard deviation is one of the widely used method to calculate dispersion of distribution but limited as in walmart_sales analysis it present value on dollar and STDDEV is higher because of higher sales. Therefore covariance gives percentages for the dispersion and calculating it. 
```sql
with store_avg_std as (SELECT -- step -1, calculating std and mean
 	store as store_number,
    ROUND(AVG(weekly_sales)::numeric, 2) AS avg_weekly_sales,
    ROUND(STDDEV(weekly_sales)::numeric, 2) AS std_dev_sales
FROM walmart_sales
group by store),
cov_sales as (Select -- step - 2 calculating covariance
store_number, 
avg_weekly_sales
std_dev_sales, 
Round((std_dev_sales/nullif (avg_weekly_sales, 0)*100) :: numeric, 2) as store_covariance_pct
from store_avg_std)
select * from cov_sales -- step 3 recalling the columns
order by store_covariance_pct desc;
```

***Results***:
* Store 35 have highest covariance means this store have most variations sales.  
