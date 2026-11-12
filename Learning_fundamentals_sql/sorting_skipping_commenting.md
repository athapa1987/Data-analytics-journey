# Using postgres dvdrental data for query. 
## sorting data: 
```sql
select * from customer
order by first_name desc;
```
*sort customer table by descending order by firs_name column.*

```sql
select 
film_id,
title,
release_year,
rental_rate
from film
order by release_year, rental_rate, film_id asc;
```
*sorting film table by release year rent and id of movie. It will first arrange the films by year
if the year are same then it will arrange by rent and also rent are same then sort in order of film_id. 
all the sorting is in ascending order. I noted the release year are all 2006.*

```sql
select 
film_id,
title,
release_year,
rental_rate
from film
order by release_year asc, rental_rate asc, film_id desc;
```
*similarly this query order in direction of release year and rental rate ascending and film_id in descending.*
```sql
select
title, 
rental_rate
from film
order by release_year desc, rental_rate asc;
```
*sorting can be done by hidden columns. As it will sort the movie from latest and from low rental rates to high.*

## Skipping data using limit, offset
```sql
select * from actor
limit 10;
```
*this can query give first 10 rows from table actor.*

```sql
select * from actor
limit 10 offset 2;
```
*Above query give 10 rows from actor table from 3rd row.*

## commenting 

```sql
-- This query returns first 10 rows from film table from 5th row.
select * from film
limit 10 offset 5;
```
*on above query first line is comment and sql omit it.*

```sql
/*This query returns 3 film_name
... with most expensive
.... excluding first 2 most expensive movies.*/
```sql
select 
film_id,
title,
release_year,
rental_rate
from film
order by rental_rate desc
limit 3 offset 2;
```

