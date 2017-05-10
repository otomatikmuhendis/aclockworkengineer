---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: Connect to Postgres with EF Core
subtitle: Postgres Notes of a .NET Developer
tags:
  - PostgreSQL
serieName: postgres
---
We can use an [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) Web Application template to create our Web API. We will use [Npgsql](http://www.npgsql.org/), a [PostgreSQL](https://www.postgresql.org/) database provider for Entity Framework Core. Install its package from NuGet with the command below.

	PM>  Install-Package Npgsql.EntityFrameworkCore.PostgreSQL

If you already have a Postgres database, add its connection string to _appsettings.json_ as _PostgreConnection_ or you can read [Installing PostgreSQL and Loading Sample Data]({{site.baseurl}}/2017/05/05/installing-postgresql-and-loading-sample-data/) post in this series to install a database on your machine.

**appsettings.json**

{% highlight json %}
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "Data": {
    "PostgreConnection": {
      "ConnectionString": "User ID=dvdclerk;Password=dvdsafe;Host=localhost;Port=5432;Database=dvdrental"
    }
  }
}
{% endhighlight %}

Write a model for film table of the sample data. Important part is Postgres is case sensitive here, so we need to define column and table names explicitly. Also EF wants to know the identity column of the table.

<!--more-->

**Models/Film.cs**

{% highlight csharp %}
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace EFCore_Postgres.Models
{
  [Table("film", Schema="public")]
  public class Film
  {
    [Column("film_id")]
    [Key]
    public int ID { get; set; }

    [Column("title")]
    public string Title { get; set; }

  }
}
{% endhighlight %}

Now we have our model so we can create our database context like below;

**ElephantContext.cs**

{% highlight csharp %}
using Microsoft.EntityFrameworkCore;

namespace EFCore_Postgres
{
  public class ElephantContext: DbContext
  {
    public ElephantContext(DbContextOptions<ElephantContext> options)
      : base(options)
    {
    }

    public DbSet<Models.Film> Films { get; set; }
  }
}
{% endhighlight %}

We have to add this context to container in `ConfigureServices` function in `Startup` class of our web application. Remember to add `Microsoft.EntityFrameworkCore` reference to usings.

**Startup.cs**

{% highlight csharp %}
using Microsoft.EntityFrameworkCore;

...

public void ConfigureServices(IServiceCollection services)
{
  // Add framework services.
  services.AddMvc();

  services.AddDbContext<ElephantContext>(options => options.UseNpgsql(Configuration["Data:PostgreConnection:ConnectionString"]));
}
{% endhighlight %}

Congratulations, it is ready to use now. Let the [built-in dependency injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection) of ASP.NET Core to initialize the database context for us in the sample `ValuesController` from Web API template.

**Controllers/ValuesController.cs**

{% highlight csharp %}
private readonly ElephantContext _context;

public ValuesController(ElephantContext context)
{
  _context = context;
}

// GET api/values
[HttpGet]
public IEnumerable<string> Get()
{
  var films = _context.Films.Select(f => f.Title).ToList();

  return films;
}
{% endhighlight %}

We will have a list of movie titles as a response.

You can find source code of the project and my notes in [olcay/EFCore_Postgres](https://github.com/olcay/EFCore_Postgres) repo.


