# Data Query Language:
## Introduction:
I am transitioning to query language and analysis. 
## Selecting the data columns, data from table
**Basic Select - selecting all the data from table**
  ```sql
  select * from employees;
  ``
** Selecting specific columns:** 
  ```sql
  select
  employees_code,
  full_name,
  salary from employees;
  ```
  ** Selecting specific information with 'WHERE'**
    ```sql
    select * from employees
    where salary >= 50000;
    ``
    

