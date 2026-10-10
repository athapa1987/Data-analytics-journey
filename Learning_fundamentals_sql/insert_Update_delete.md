## Introduction: 
Here in this document I inserted data and updated table. Furthermore, I created the other table for data integrity and automation. So, this file is detailed practice of data manipulating language (DML).  

## 1. Insert data into the table employee
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
   
## 2. Experimenting the default values are working or not:
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

* I could maintain the records but there is a problem as the salary history is not updated automatically. Thus, I decided re-engineering it
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
  
```sql
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

```sql
create trigger trg_salary_history 
after update on employees 
for each row 
execute function log_salary_increment ();
```

### to try the function is successful or not - 
lets assume due to great performance from marketing team management decided to increase the salary of marketing department by 2000. 
```sql
Update employees
set salary = salary + 2000 
where department = 'Marketing';
```
### to check whether it is done automatically or not
```sql
select * from salary_history sh
join employees e
on sh.employees_id = e.employees_id
where e.department = 'Marketing'
order by e.employees_id;
```
* trigger worked successfully as salary is updated 2 times on marketing.

### Further to see how much salary increased each time and verifying the audit log. Also wanted to salary grouped by department and employees_id. Since group by and order by might confuse the command and thus,

```sql
select
sh.history_id,
e.employees_id,
e.first_name ||' '||e.last_name as full_name,
e.department,
sh.old_salary,
sh.new_salary,
round(sh.new_salary - sh.old_salary, 2) as increment_amound
FROM salary_history sh
JOIN employees e ON sh.employees_id = e.employees_id
order by e.department, sh.employees_id;
```
* Result: it displayed the table arrange by department and employees.

### what if the new employees are added and they have no salary history, it can be seen using left join however, we also could add function to do this, 
* creating function 
```sql
create or replace function log_employee_audit ()
returns trigger as $$
begin
insert into salary_history (employees_id, old_salary, new_salary)
values (new.employees_id, 0.00, new.salary);
return new;
end; 
$$ language plpgsql;
```

* executing function:
```sql
create trigger trg_employee_audit
after insert on employees 
for each row
execute function log_employee_audit ();
```sql

* so to try the function
```sql
insert into employees (first_name, last_name, email, department, salary)
Values
('Hari','Shanker','shanker.hari@example.com','Marketing','34000');
```
* Now checking if new employee is added to other table
```sql
select * from salary_history sh
join employees e
on sh.employees_id = e.employees_id
where e.first_name = 'Hari' and e.last_name = 'Shanker';
```
Verified: it worked and updated successfully. 

* whatif somebody changed the data for their own benefit, so that updating employees table
```sql
alter table employees
add column created_at timestamp default current_timestamp, 
add column created_by varchar(50) default current_user, 
Add column updated_at timestamp, 
add column updated_by varchar (50);
```
Updating table 
```sql
set updated_at = current_timestamp, 
updated_by = 'Initial setup' 
where updated_by is null; 
```
* so the current user, updated_by, updated_at will remain postgres as I did not make any log in or role and login passwords yet.

For salary history table 
```sql
alter salary_history
add column updated_by varchar (50);
```
Bringing the data back to the salary history 
```sql
UPDATE salary_history sh
SET updated_by = e.updated_by
FROM employees e
WHERE sh.employees_id = e.employees_id;
```
