---
author: Olcay Bayram
layout: post
categories: Projects
published: true
title: BlazorBin
image: /img/blazorbin.png
tags:
  - azure
  - signalr
  - function
  - blazor
subtitle: Yet another request bin
---

Yet another request bin but made with Blazor this time. When you build serverless apps or functions, your system depends on webhooks a lot. There are request bin tools on web which provides you unique endpoints so you can send requests from your system and see what is coming in  the payload.

When I saw [Blazor WebAssembly 3.2.0 Preview 1 release now available](https://devblogs.microsoft.com/aspnet/blazor-webassembly-3-2-0-preview-1-release-now-available) article on ASP.NET Blog, I decided to give it a try to work with Azure SignalR Service.

~~BlazorBin is on Azure and can be accessed by blazorbin.azurewebsites.net/~~. **This service does not continue because of a billing issue.** A new endpoint to an Azure Function, will be created when you visit the website. When a request comes to the endpoint, it will be listed on the left-hand-side menu as the method name. You can see the details of that request by clicking on the menu item.

<!--more-->

![solution_structure.png]({{site.baseurl}}/img/solution_structure.png)

The hardest part in this project was using multiple Azure SignalR Services because of limitations. You need two SignalR services, one in Default and the other one in Serverless service mode. You can see the comparions [here](https://github.com/Azure/azure-signalr/blob/dev/docs/faq.md#what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose). Since Blazor has the hub server, we need a SignalR service in Default mode for it. Another one in Serverless mode to communicate between clients. The clients are an Azure Function and a Blazor app in our case.

The code can be found on [github.com/olcay/blazorbin](https://github.com/olcay/blazorbin) repository. I tried to explain how to install it step by step. I used Github Actions to deploy to Azure. It is running on free resources. I have been using it to test ingrations of my serverless systems for a while now.
