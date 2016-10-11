---
author: Olcay Bayram
layout: post
title: Run ASP.NET Core Websites with Command Line Arguments
categories: Software
tr: /2016/10/11/aspnet-core-uygulamasini-parametreyle-calistirmak/
tags: 
  - aspnet core
  - commandline
  - dotnet
published: true 
---

The DLL file that contains [ASP.NET Core](https://www.asp.net/core) application can run on [KestrelHttpServer](https://github.com/aspnet/KestrelHttpServer) without the need to IIS. When you run it with [dotnet cli](https://github.com/dotnet/cli), it works like a console application.

So we want to deploy it to [Heroku](https://www.heroku.com/) cloud platform to see its cross-platform capabilites.

Heroku app engine runs the dotnet cli with the parameters below.

`cd /app/heroku_output && dotnet ./Libton.dll --server.urls http://+:54372`

The `server.urls` parameter sets the port variable which can be different everytime. The default value is 5000 in ASP.NET Core and our application doesn't have the capability to read the command line arguments so it won't set and listen the specified port.

In order to give this capability to our application, we could add the reference of [Microsoft.Extensions.Configuration.CommandLine](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.CommandLine/) via NuGet Package Manager.

Now, we could convert the command line arguments to configuration settings and use them on the web host builder on Main method of the Program class.

Finally, the Program.cs file will look like this;

<!--more-->

{% highlight csharp linenos %}
using System.IO;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;

namespace Libton
{
  public class Program
  {
    public static void Main(string[] args)
    {
      //Builds configuration settings out of command line arguments
      var config = new ConfigurationBuilder()
        .AddCommandLine(args)
        .Build();

      var host = new WebHostBuilder()
        .UseConfiguration(config)//Uses the configuration settings
        .UseKestrel()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseIISIntegration()
        .UseStartup<Startup>()
        .Build();

      host.Run();
    }
  }
}
{% endhighlight %}

Yes, ASP.NET works on Heroku.