---
title: Copy Database with Backup on SQL Management Studio
author: Olcay Bayram
layout: post
tr: /2013/08/28/sql-management-studio-backup-ile-database-kopyalama/
categories:
  - Database
tags:
  - database
  - management studio
  - mssql
  - sql
  - backup
---
We can get a copy of any database with Backup and Restore commands in SQL Server Management Studio but there are a few tricks which makes our life easier.

First of all, it must be a full backup because a differential backup would be useless.

We can not restore a database to another one. If you just want to change the name of the database, you can easily give a new name at the restore process. 

<!--more-->

If you receive an error which says that the database is in transition, after the restore process, the simplest method is to close the SQL Management Studio.

The newly restored database may still be using the old Service Broker. It won't show any errors but it may be annoying during debug.
In this case you must stop all operations on the database to define a new broker.

{% highlight sql linenos %}
USE master
go

DECLARE @dbname sysname

SET @dbname = 'NewDatabaseName'

DECLARE @spid int
SELECT @spid = min(spid) from master.dbo.sysprocesses where dbid = db_id(@dbname)
WHILE @spid IS NOT NULL
BEGIN
EXECUTE ('KILL ' + @spid)
SELECT @spid = min(spid) from master.dbo.sysprocesses where dbid = db_id(@dbname) AND spid > @spid
END

ALTER DATABASE [NewDatabaseName] SET NEW_BROKER;
{% endhighlight %}

### The database is in use

We can also turn the database offline to avoid 'the database is in use' error.

`Exclusive access could not be obtained because the database is in use.`

{% highlight sql linenos %}
--Run this part before restore
use master
alter database [DatabaseName] set offline with rollback immediate;

--Run this part after restore
use master
alter database [DatabaseName] set online with rollback immediate;
{% endhighlight %}