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
**Selecting specific information with `WHERE` using other operators `AND`, `OR`, `NOT`, `IN`, `NOT IN` `LIKE`, `ILIKE`**

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
*Retrieving data two departments IT and Finace*
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
**Tips** - It is better to use 'In' or 'not in' while choosing information from same column instead of using multiple `or`, `and` and `not` conditions. 

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
*Viewing 10 employees on the top of the list
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


  


