---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: .NET Core Runtime Image on Docker
tags:
  - docker
  - dotnet
  - .NET Core
---
Let's try dotnet sample application within a docker container.

	docker run microsoft/dotnet-samples
    
![docker_microsoft_sample.png]({{site.baseurl}}/img/docker_microsoft_sample.png)

<!--more-->

If we have a working docker, we can build our image. First, we will create a dotnet console application;

	mkdir dotnetapp
    cd dotnetapp
    dotnet new console
    dotnet restore
    dotnet publish -c Release -o out
   
![docker_dotnetapp.png]({{site.baseurl}}/img/docker_dotnetapp.png)

We need a `Dockerfile` to declare the dependency on a .NET Core runtime image for our application. It will be like this;

    FROM microsoft/dotnet:runtime
    WORKDIR /dotnetapp
    COPY out .
    ENTRYPOINT ["dotnet", "dotnetapp.dll"]
    
Now, we can build the image and run it.

	docker build -t dotnetapp .
	docker run -it --rm dotnetapp
    
![docker_build_dotnetapp.png]({{site.baseurl}}/img/docker_build_dotnetapp.png)

We have a result message "Hello World!" printed out on the console and we can see our image installed in docker images list.

	docker images -a
