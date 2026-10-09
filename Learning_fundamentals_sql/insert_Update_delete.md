# This files includes methods of inserting, updating and deleting data

## 1. Insert data on to the table employee
```sql
INSERT INTO employees (First_name, Last_name, Email, Department, Salary, Start_date)
VALUES
    ('Priya', 'Singh', 'priya.singh@example.com', 'HR', 45000.00, '2019-03-22'),
    ('Arjun', 'Verma', 'arjun.verma@example.com', 'IT', 55000.00, '2021-06-01'),
    ('Suman', 'Patel', 'suman.patel@example.com', 'Finance', 60000.00, '2018-07-30'),
    ('Kavita', 'Rao', 'kavita.rao@example.com', 'HR', 47000.00, '2020-11-10'),
    ('Amit', 'Gupta', 'amit.gupta@example.com', 'IT', 52000.00, '2020-09-25'),
    ('Neha', 'Desai', 'neha.desai@example.com', 'Marketing', 48000.00, '2019-05-01'),
    ('Rahul', 'Kumar', 'rahul.kumar@example.com', 'IT', 53000.00, '2002-01-01'),
    ('Anjali', 'Mehta', 'anjali.mehta@example.com', 'Finance', 61000.00, '2006-03-01'),
    ('Vijay', 'Nair', 'vijay.nair@example.com', 'Marketing', 50000.00, '2025-04-26'),
    ('Raj', 'Sharma', 'raj.sharma@example.com', 'IT', 80000.00, '2001-01-01'),
    ('Sarah', 'Connor', 's.connor@tech.com', 'IT', 75000.00, '2025-12-22'),
    ('Marcus', 'Wright', 'm.wright@tech.com', 'Engineering', 68000.00, '2025-12-22'),
    ('Kyle', 'Reese', 'k.reese@tech.com', 'IT', 52000.00, '2025-12-22'),
    ('Grace', 'Harper', 'g.harper@tech.com', 'HR', 48000.00, '2025-12-22'),
    ('Dani', 'Ramos', 'd.ramos@tech.com', 'Marketing', 55000.00, '2025-12-22');
```
* Employee id is not inserted as it is automatically generated starting from 1234 and increasing 104.
   
## 2. Inserting more data
```sql
INSERT INTO employees (First_name, Last_name, Email, Department)
VALUES 
    ('Deepa', 'Gandhi', 'alex.taylor@example.com', 'Operations'),
    ('Aman', 'Thapa', 'jordan.lee@example.com', 'Marketing');
```
* As the minimum salary is default as 30000 and start date is default as current date, thus those data were not put.

## 3. Updating Table: 
lets assume salary is increased by 2% this year to all the employees. Therefore, I use

* Here is the problem, it will update salary by 2% which is current salary but will loose the old record. Therefore, downloading the file on csv is one thing we could do but it is prone to human error and storing and retrieving problem. Therefore, it looks better to create another table and keep record the histroy. 

```sql
create table salary_history (
history_id bigint primary key generated always as identity, 
employees_id bigint references employees (employees_id),
old_salary Decimal (10, 2) not null, 
new_salary decimal (10,2) not null,
changed_at timestamp default current_timestamp
);
```
* history_id is kept as primary key and connected to the identity thus it makes easy to see the updates of salaries, also kept the exact time (current_timestamp).
  
Then inserting salary and employee into the table
```sql
insert into salary_history (employees_id, old_salary, new_salary)
select 
employees_id, 
salary as old_salary, 
round(salary * 1.02, 2) as new_salary
from employees; 
```

* Now I update the main table maintaining the history.

```sql
update employees
set salary = Round(salary *1.02, 2);
```

*    I could maintain the records but there is a problem as the salary history is not updated automatically. Thus, I decided re-engineering it
  Thus dropping the salary_history table.
```sql
drop table salary_hisotry;
```
* Creating salary_history table again now,
  
```sql
create table  if not exists salary_history (
history_id bigint primary key generated always as identity, 
employees_id bigint not null, 
old_salary decimal (10, 2) not null, 
new_salary decimal (10, 2) not null, 
changed_at timestamp default current_timestamp, 
constraint fk_salary_history_employees 
foreign key (employees_id) 
references employees (employees_id));
```
* Here I think I waisted time as I made the same table as the former salary_histroy table have automatic foreign key constraint assigned to employees as reference from table directly while here I manually did.
* Also I could put 'on delete cascade' if I want to remove the detail of employees that are no longer in system or 'on update cascade' if I want to update the table but my purpose is to keep the history thus I did not use them.
* Now repeating the process of putting data again but the salary table is already updated with 2% thus,
```sql
Insert into salary_history (employees_id, old_salary, new_salary)
select 
employees_id, 
round(salary/1.02)as old_salary,
salary as new_salary 
from employees;
```
* Now I came to the same place where I wanted to automate the salary history log thus I create function now
  
``sql
create or replace function log_salary_increment()
returns trigger as $$
begin
if old.salary is distinct from new.salary then 
insert into salary_history (employees_id, old_salary, new_salary)
values (old.employees_id, old.salary, new.salary);
end if;
return new; 
end; 
$$language plpgsql;
```
* now creating and attaching function
### dropping if there is any function 
```sql
drop trigger if exists trg_salary_history on employees;
```
### now creating function for automatic salary_history update
create trigger trg_salary_history 
after update on employees 
for each row 
execute function log_salary_increment ();




