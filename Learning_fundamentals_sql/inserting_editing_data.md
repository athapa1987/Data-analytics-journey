# This files includes insert and edit of data
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


