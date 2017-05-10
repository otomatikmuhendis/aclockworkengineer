---
author: Olcay Bayram
layout: post
categories: Database
published: true
title: Installing PostgreSQL and Loading Sample Data
subtitle: Postgres Notes of a .NET Developer
tags:
  - PostgreSQL
serieName: postgres
---
As an experienced and certified .NET Developer, Postgres made my attention. I will tell about its reasons in another post.

I will write my notes about Postgres in this series. We will start with installing and loading sample data to our database in this post and we will have a result like below at the end.

![postgres_dvdrental.png]({{site.baseurl}}/img/postgres_dvdrental.png)

First, download PostgreSQL database from [PostgreSQL Downloads](https://www.postgresql.org/download/) page to install. I use Windows so I chose Windows installer but you can use another operating system and also to develop because we can code ASP.NET Core Web Application in different operating systems.

<!--more-->

Add PostgreSQL bin folder to Windows Path in Environment Variables.

`C:\Program Files\PostgreSQL\9.6\bin`

Open up a Command Prompt window and write those methods to create the database and the user of it.

	> createdb dvdrental

	> create user dvdclerk with password 'dvdsafe'; 
	> grant all privileges on database dvdrental to dvdclerk;

We have the database server and the database ready to load some data. We will restore sample database from [PostgreSQL Tutorial](http://www.postgresqltutorial.com).

Download [DVD Rental Sample Database](http://www.postgresqltutorial.com/postgresql-sample-database/) file and unzip it to your C:\ drive. Call `pg_restore` command to restore it.

`> pg_restore -d dvdrental C:\dvdrental.tar`

Run the *_psql_* application to connect our new fully loaded database.

`> psql dvdrental`

Now we can display the list of the tables with the command below;

`dvdrental=# \dt`

If you have installed it succesfully, continue with [Connect to Postgres with EF Core]({{site.baseurl}}/2017/05/05/installing-postgresql-and-loading-sample-data/)

Source:

- [Postgres for .NET Developers](https://app.pluralsight.com/library/courses/postgres-dotnet-developers/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/current/static/)
- [PostgreSQL Tutorial](http://www.postgresqltutorial.com/)
- [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/)

