## Introduction:
As standard deviation is one of the widely used method to calculate dispersion of distribution but limited as in walmart_sales analysis it present value on dollar and STDDEV might be high because of higher sales. Therefore coefficient of variance gives percentages for the dispersion and calculating it. so that it makes easier to compare which stores have more variations on sales or there are high ups and downs in sales.
```sql
with store_avg_std as (SELECT -- step -1, calculating std and mean
 	store as store_number,
    ROUND(AVG(weekly_sales)::numeric, 2) AS avg_weekly_sales,
    ROUND(STDDEV(weekly_sales)::numeric, 2) AS std_dev_sales
FROM walmart_sales
group by store),
cv_sales as (Select -- step - 2 calculating coefficients of variation
store_number, 
avg_weekly_sales
std_dev_sales, 
Round((std_dev_sales/nullif (avg_weekly_sales, 0)*100) :: numeric, 2) as store_cv_pct
from store_avg_std)
select * from cov_sales -- step 3 recalling the columns
order by store_cv_pct desc;
```

***Results***:
* Store 35 have highest covariance means this store have most variations sales.  
