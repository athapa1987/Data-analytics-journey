## Using Postgres sample data (dvdrental) to practice queries. 
  There were 15 tables. Using terminal to view detail of columns, foreign key to use joins and other queries. 
``` sql
select * from actor
```
*This query gives all the rows from actor table.* There are 200 actor list. 

```sql
select first_name,
last_name from actor;
```
*asking to two columns first and last name from actor table.*

```sql
select
first_name as customer_name
from customer;
```
*renaming the column name as..*
