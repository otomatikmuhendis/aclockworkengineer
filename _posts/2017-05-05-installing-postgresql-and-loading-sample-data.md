---
author: Olcay Bayram
layout: post
categories: Database
published: true
title: Installing PostgreSQL and Loading Sample Data
subtitle: Postgres Notes of a .NET Developer
tags:
  - PostgreSQL
---
### Postgres Notes of a .NET Developer

As an experienced and certified .NET Developer, Postgres made my attention. I will tell about its reasons in another post.

I will write my notes about Postgres in this series. First, let's start with installing the PostgreSQL database.

Download it from [PostgreSQL Downloads](https://www.postgresql.org/download/) page. I use Windows so I chose Windows installer but you can use another operating system to install and also to develop because we can code ASP.NET Core Web Application in different operating systems.

Add PostgreSQL bin folder to Windows Path in Environment Variables.

`C:\Program Files\PostgreSQL\9.6\bin`

Open up a Command Prompt window and write those methods to create the database and the user of it.

`> createdb dvdrental`

`> create user dvdclerk with password 'dvdsafe'; 
> grant all privileges on database dvdrental to dvdclerk;`

We have the database server and the database ready to load some data. We will restore sample database from [PostgreSQL Tutorial](http://www.postgresqltutorial.com).

Download [DVD Rental Sample Database](http://www.postgresqltutorial.com/postgresql-sample-database/) file and unzip it to your C:\ drive. Call `pg_restore` command to restore it.

`> pg_restore -d dvdrental C:\dvdrental.tar`

Run the *_psql_* application to connect our new fully loaded database.

`> psql dvdrental`

Now we can display the list of the tables with the command below;

`dvdrental=# \dt`

![postgres_dvdrental.png]({{site.baseurl}}/img/postgres_dvdrental.png)

If you have installed it succesfully, continue with [Connect to Postgres with EF Core]()
