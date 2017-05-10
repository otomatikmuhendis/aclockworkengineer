---
author: Olcay Bayram
layout: post
categories: Database
published: true
title: PostgreSQL as NoSQL
subtitle: Postgres Notes of a .NET Developer
tags:
  - PostgreSQL
serieName: postgres
---
This post is continuation of the series. If you do not have Postgres on your machine or sample data, you can read [Installing PostgreSQL and Loading Sample Data]({{site.baseurl}}/2017/05/05/installing-postgresql-and-loading-sample-data/) post first.

There are two JSON (JavaScript Object Notation) types for storing JSON data.

> The json data type stores an exact copy of the input text, which processing functions must reparse on each execution; while jsonb data is stored in a decomposed binary format that makes it slightly slower to input due to added conversion overhead, but significantly faster to process, since no reparsing is needed.

Create a table with a column with `jsonb` data type.

	create table film_docs(data jsonb);

Insert all the data in the row to that column. The original table comes from the sample data.

	insert into film_docs(data)
	select row_to_json(film)::jsonb
	from film;

	select (data ->> 'title') as Title,
	(data -> 'length') as Length
	from film_docs
	where data -> 'title' ? 'Chamber Italian';

There are lots of [JSON functions and operators](https://www.postgresql.org/docs/current/static/functions-json.html). I think Entity Framework does not have these functions. <!--more-->I will search for this next time.

	explain select (data ->> 'title') as Title,
	(data -> 'length') as Length
	from film_docs
	where data @> '{"title": "Chamber Italian"}';

`jsonb` also supports indexing, which can be a significant advantage. Here we have an explanation of the above query without index.
	
	Seq Scan on film_docs  (cost=0.00..96.50 rows=1 width=64)
	  Filter: (data @> '{"title": "Chamber Italian"}'::jsonb)
	Time: 0.009s

If we create a GIN index and run `explain` statement again, it would give us less cost. `jsonb_path_ops` means generated index will be specific for `@>` operator.

create index on film_docs using GIN(data jsonb_path_ops)

Bitmap Heap Scan on film_docs  (cost=12.01..16.03 rows=1 width=64)
  Recheck Cond: (data @> '{"title": "Chamber Italian"}'::jsonb)
  ->  Bitmap Index Scan on film_docs_data_idx  (cost=0.00..12.01 rows=1 width=0)
        Index Cond: (data @> '{"title": "Chamber Italian"}'::jsonb)
Time: 0.008s

If you want to continue with more advanced topics on PostgreSQL, search for table inheritance.