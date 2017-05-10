---
author: Olcay Bayram
layout: post
categories: Database
published: true
title: PostgreSQL Advanced
subtitle: Postgres Notes of a .NET Developer
tags:
  - PostgreSQL
serieName: postgres
---
This post is continuation of the series. If you do not have Postgres on your machine or sample data, you can read [Installing PostgreSQL and Loading Sample Data]({{site.baseurl}}/2017/05/05/installing-postgresql-and-loading-sample-data/) post first.

There are functions like `date_gt` to compare date types.

	select * from payment
	where date_gt(payment_date::date, '2007-04-01'::date);
	
You can use `date_part` to get a part of datetime.

	select amount, payment_date, date_part('quarter', payment_date) as quarter, 
    date_part('year', payment_date) as year, concat('Q', date_part('quarter', payment_date), '-',
    date_part('year', payment_date)) as display_quarter
	from payment;

[generate_series(start, stop, step)](https://www.postgresql.org/docs/current/static/functions-srf.html) generates a series of values, from `start` to `stop` with a step size of `step`. We need a function like `f(x)` to get the value. If we use a negative number as `step`, it will be decreasing.

	select x, md5(random()::text) from generate_series(100, 0, -5) as f(x);

	select x from generate_series('2001-10-01'::TIMESTAMP, '2002-10-01'::TIMESTAMP, '10 days') as f(x);
	
![postgres_generate_series.png]({{site.baseurl}}/img/postgres_generate_series.png)

<!--more-->

	select trunc(random() * 1000 + 1) from generate_series(1, 1000);
	
Let's use this series in an example;
	
	create or replace function random_payments(counter int) returns setof payment
	as $$
	DECLARE
		start_id int;
		end_id int;
	BEGIN
		select min(payment_id) from payment into start_id;
		select max(payment_id) from payment into end_id;
		return query (select * from payment
		where payment_id IN(
		select trunc(random() * (end_id - start_id) + start_id)
		from generate_series(1, counter + 1)));
	END
	$$ language plpgsql;

	select * from random_payments(100);
	
If you have large and complicated queries, you can use `WITH` query to make it easily readable. These statements often referred to as [CTE](https://www.postgresql.org/docs/current/static/queries-with.html) (Common Table Expresssions). The subquery also generates a tsvector to query but I have already mentioned that before [it is a bad practice]({{site.baseurl}}/2017/05/09/postgresql-introduction/).

	with actor_rollup as (select f.film_id, title, sum(p.amount)::money as total_sales,
	group_concat(first_name || ' ' || last_name) as actors,
	to_tsvector(concat(f.title, ' ', group_concat(first_name || ' ' || last_name))) as search_field
	from film f
	inner join film_actor fa on f.film_id = fa.film_id
	inner join actor a on a.actor_id = fa.actor_id
	inner join inventory i on i.film_id = f.film_id
	inner join rental r on r.inventory_id = i.inventory_id
	inner join payment p on p.rental_id = r.rental_id
	group by f.film_id, title)

	select title, actors, total_sales
	from actor_rollup
	where search_field @@ to_tsquery('jedi');

We can use CTEs with Views together but they are different. CTEs are disposed when the query is done. Views are database wide and can have permissions. CTEs can be recursive.

	create view raw_sales as
	select title, description, length, rating, p.amount, payment_date,
	date_part('quarter', payment_date) as quarter,
	date_part('month', payment_date) as month,
	date_part('year', payment_date) as year,
	concat('Q', date_part('quarter', payment_date)::text, '-', date_part('year', payment_date)::text) as qyear,
	cash_words(amount::money) as spelling_it_out,
	to_tsvector(concat(title, ' ', description)) as search_field
	from film f
	inner join inventory i on i.film_id = f.film_id
	inner join rental r on r.inventory_id = i.inventory_id
	inner join payment p on p.rental_id = r.rental_id;


	with sales_rollups as (select distinct title, qyear,
		sum(amount) over (partition by title, qyear) as "Quarterly Sales",
		sum(amount) over (partition by qyear) as "Total Quarterly",
		sum(amount) over (partition by title, qyear)/sum(amount) 
        over (partition by qyear) * 100 as "Percent of Total Quarter"
		from raw_sales
	order by title),
		q1_2007 as (select * from sales_rollups where qyear = 'Q1-2007'),
		q2_2007 as (select * from sales_rollups where qyear = 'Q2-2007'),
		things_are_correct as (
			select 
				(select sum("Percent of Total Quarter") from q2_2007),
				(select sum("Percent of Total Quarter") from q1_2007)
	)

	select * from things_are_correct

We will continue with NoSQL features of PostgreSQL next time.
