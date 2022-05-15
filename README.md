# SQL-Assignment-3
SQL Assignment 3

5.3.1: What are all the records in the store table, ordered by the primary key of the table?
QUERY : SELECT * FROM store order by store_id;

5.3.2: What are the first 10 records in the inventory table in ascending order of update (show just the primary key and the update timestamp)?
QUERY : SELECT inventory_id, last_update FROM inventory order by last_update ASC limit 10;

5.3.3: What is the street address, district, city name, and phone number for all addresses in the 18743 postal code?
QUERY : select a.address as "Street Address", a.district, x.city, a.phone number
        from address as a
        inner join city as x on x.city_id = a.city_id
        where a.postal_code = '18743';

5.3.4: How many records are in the inventory table?
QUERY:	select count(*)  from inventory; 

5.3.5: What is the timestamp of the most recent update to the staff table?
QUERY:	select last_update from staff order by last_update desc limit 1;

5.3.6: How many distinct values of country_id exist in the city table?
QUERY: select distinct count(country_id) from city;

5.3.7: How many actors' last names begin with either a "B" or a "C"?
QUERY: select count(last_name) from actor where last_name like 'B%' or last_name like 'C%';

5.3.8: What is the average payment amount (rounded to the nearest cent) and number of payments made in the month of April, 2007, by customers that are currently active, and whose average payment is at least $5.00?
QUERY: select a.customer_id, b.active,
       ROUND(avg(a.amount),2), count(*)
       from payment AS a
       inner join customer AS b ON a.customer_id=b.customer_id
       where cast(a.payment_date AS DATE)
       between '2007-04-01' and '2007-04-30'
       and b.active=1
       group by a.customer_id, b.active
       having avg(a.amount) >= 5.00;

5.3.9: Show customer's first and last name and email, and label the average payment field as "Avg. Payment Amount" and the number of payments field as "Number of Payments".
QUERY:	select a.first_name, a.last_name, a.email, avg(b.amount) as "Avg. Payment Amount",
		    count(b.amount) as "Number of Payments"
		    FROM customer as a
		    inner join payment as b ON a.customer_id=b.customer_id
		    group by a.customer_id, b.amount
        
5.3.10: Order the results by highest payment first, and then by last name.
QUERY: select a.amount, b.last_name
		   from payment as a
		   inner join customer as b ON a.customer_id = b.customer_id
		   group by  a.amount, b.last_name
		   order by a.amount desc;

5.3.11: What are all the first names of both the actors and the staff members that begin with the letter 'S' (in alphabetical order)?
QUERY: select first_name from actor
       where first_name like 'S%'
       UNION ALL
       select first_name from staff
       where first_name like 'S%'
       order by first_name asc

5.3.12: Show the title of the films that Jon rented on May 29, 2005 in common with those that Mike rented on June 20, 2005. Order by film title.
QUERY: select f.title from film as f
       inner join inventory as i on i.film_id=f.film_id
       inner join rental as r on r.inventory_id=i.inventory_id
       inner join customer as c on c.customer_id=r.customer_id
       where c.first_name='Jon' and cast (r.rental_date as                       DATE)='2005-05-29'
       union all
       select f.title from film as f
       inner join inventory as i on i.film_id=f.film_id
       inner join rental as r on r.inventory_id=i.inventory_id
       inner join customer as c on c.customer_id=r.customer_id
       where c.first_name='Mike' and cast (r.rental_date as DATE)='2005-06-20'
