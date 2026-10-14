# Data Query Language:
## Introduction:
I am transitioning to query language and analysis. 
## Selecting the data columns, data from table
**Basic Select - selecting all the data from table**
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
  **Selecting specific information with 'WHERE'**
  *employees having salary more than or equal to 50000.*
    ```sql
    select * from employees
    where salary >= 50000;
    ```
    *salary more than 500000 'AND' department is IT.*
    ```sql
    select * from employees
  where salary > 50000 and department = 'IT';
   ```
* Only viewing data of two departments IT and Finace*
```sql
 select * from employees
  where department = 'IT' or department = 'Finance';
```

  *Finding out distinct departments (how many departments are there)*
  ```sql
  select distinct department from employees;
  ```
  *retrieving information using 'SELECT' and 'LIKE'

  - Employee name starting form A
```sql
  select * from employees
  where full_name like 'A%';
```

 - finding the employees with name 'ILIKE' (using case insensitive)
```sql
select * from employees
  where full_name ilike 'arjun%';
```

  


