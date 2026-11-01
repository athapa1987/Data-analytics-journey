# Data Query Language:
## Introduction:
I am transitioning to query language and data analysis. I documented the queries I make in brief. I started from select, then use filter, order, group and aggregate functions. Also, used all them together as I need. 

## Selecting the data columns, data from table
**Basic `SELECT` - selecting all the data from table**
  ```sql
  select * from employees;
  ```
**Selecting specific columns**
```sql
  select
  employees_code,
  full_name,
  salary from employees;
  ```
## Filtering:
**Filtering or retrieving specific information with `WHERE` using other operators `AND`, `OR`, `NOT`, `IN`, `NOT IN` `LIKE`, `ILIKE`**

*employees having salary more than or equal to 50000.*
```sql
select * from employees
where salary >= 50000;
```

*salary more than 500000 `AND` department is IT (Both conditions need to be true- 'and').*
```sql
select * from employees
where salary > 50000 and department = 'IT';
```

*Finding the salary of employees more than 50000 or the department is HR (any condition is true `or`)
```sql
select * from employees
where salary > 50000 or department = 'HR'
```

*Using 'not' if the condition is false (finding out other department than marketing) 
```sql
select * from employees
where not department = 'HR'
```
*Retrieving data of two departments IT and Finace*
```sql
 select * from employees
  where department = 'IT' or department = 'Finance';
```
or using `IN`
```sql
SELECT * FROM employees
WHERE department in ('IT', 'Finance');
```
*Finding department other than IT and Finance using `not in`*
```sql
SELECT * FROM employees
WHERE department not in ('IT', 'Finance');
```
     or only using `not`
```sql
select * from employees
  where not department = 'IT' and not department = 'Finance';
```
**Tips** - It is better to use 'In' or 'not in' while choosing information instead of using multiple `or`, `and` and `not` conditions. 

*Finding out `distinct` departments (how many departments are there)*
```sql
select distinct department from employees;
```

*retrieving information using `SELECT` and `LIKE`*
- Employee name starting form A
```sql
  select * from employees
  where full_name like 'A%';
```

- finding the employees with name `ILIKE` (using case insensitive)
```sql
select * from employees
  where full_name ilike 'arjun%';
```

## Sorting and Limiting
**`ORDER BY` - To arrange data according to our need**
*highest paid employees to lowest paid**
```sql
select * from employees
order by salary desc; 
```
*lowest paid employees to highest paid**
```sql
select * from employees
order by salary; 
```
*ordering by department in ascending order*
```sql
select * from employees
order by department asc;
```
- we can omit asc as if we don't put anything it will be arranged in ascending order

**'LIMIT` - to view specific number of rows**
*Viewing 10 employees on the top of the list*
```sql
select * from employees
Limit 10;
```

*To see top highest paid staffs*
```sql
select * from employees
order by salary desc
limit 5;
```

## Join: 
**`Inner Join` or `Join`**
*Viewing the last salary and the current salary of the employees*
```sql
select distinct on (e.employees_id) /*used distinct on employees_d because I want to see latest growth and if I don't do this and order by employee_id it will not fulfil my purpose.*/
e.employees_id
, e.full_name
, e. employees_code
, e. department
, sh. old_salary
, e. salary
from employees e
join salary_history sh
on e.employees_id = sh. employees_id
order by e.employees_id, sh.updated_on desc; /* I ordered updated on descending
because to see the latest salary change
otherwise it will see on ascending order which will show the old salaries */
```
*Salary history and salary increased in IT department*
```sql
select
e.employees_id
, e.full_name
, e. employees_code
, sh. old_salary
, e. salary
from employees e
join salary_history sh
on e.employees_id = sh. employees_id
where department = 'IT'
order by e.employees_id, sh.updated_on;
```
**Note**: I did not used `left join`, `right join` and `outer join`. `Left join` shows all the matching rows and rows in the left (table used in from) and `right join` shows the matching rows plus information on join table while `outer join` shows all the matching rows plus other rows from all tables. 

## `Group by`:
*How many times salary was increased?*
* Using both join and group by to see it and using aggregate function count ()
```sql
select
e. employees_id
, e. full_name
, count (sh. old_salary) as number_of_increases
from employees e
join salary_history sh
on e.employees_id = sh. employees_id
group by e. employees_id, e.full_name
order by e. employees_id;
```
*to view the How many times salary was increased where clause could be added*
```sql
select
e. employees_id
, e. full_name
, count (sh. old_salary) as number_of_increases
from employees e
join salary_history sh
on e.employees_id = sh. employees_id
where e.department = 'IT'
group by e. employees_id, e.full_name
order by e. employees_id;
```
**Note:** It can be always added filtering lines to retrieve information on queries. Group by put rows together on the basis of group by is used and perform any calculation based on group by command.

*`Group by` and `having`*
```sql
SELECT 
    department, 
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
Having avg (salary) > 50000;
```
*  Because the `where` clause cannot accept aggregate functions (like `avg` or `sum`), combining `group by` with `having` is the correct way to filter grouped data using operators.

*If we need to calculate an average or other aggregate function while keeping all individual row details visible, we use the window function `over (partition by ...)`. `Group by` collapses rows into summary cards, whereas `partition by` computes the group aggregate side-by-side while preserving every individual row.*
```sql
SELECT 
full_name,
department,
salary,
Round (AVG(salary) OVER (PARTITION BY department), 2) AS dept_avg_salary, -- calculating average salary by department
RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS salary_rank_in_dept -- ranking and ordering descending
FROM employees;
```

# Windows function
## partition by:
lets assume we need to average salary, total salary and how many employees are there in every department. Using group by collapse the rows so `over (partition by ...)` is useful function in this instance. 
```sql
select full_name,
department,
count(employees_id) over (partition by department) as employees_per_department,
sum(salary) over (partition by department) as total_salary_per_department,
round(avg (salary) over (partition by department order by department), 2) as average_salary_per_department
from employees;
```

# Dates: 
```sql
set datestyle = 'iso, ymd'; -- setting date format
```
**If I want to know how many employees were worked**

SELECT full_name, 
extract(year from start_date) AS start_year,
extract (year from current_date) - extract (year from start_date) as years_worked
from employees;
using extract, in following I wanted to know only years and when they started
```sql
SELECT full_name, 
       EXTRACT(YEAR FROM start_date) AS start_year,
       EXTRACT(YEAR FROM AGE(CURRENT_DATE, start_date)) AS years_worked 
FROM employees;
```
I calculated the days they work and converted to year and value in years in decimal
```sql
SELECT full_name, 
       EXTRACT(YEAR FROM start_date) AS start_year,
       ROUND((CURRENT_DATE - start_date) / 365, 1) AS years_worked
FROM employees
```
to find out exact year, month and days of work 
```sql
SELECT full_name, 
       start_date,
       AGE(CURRENT_DATE, start_date) AS time_worked
FROM employees;
```


