---
author: Olcay Bayram
layout: post
categories: System
published: true
title: New Octopus Deploy Step
tags:
  - deployment
  - octopus
  - medianova
  - cdn
  - continuous delivery
tr: /2017/01/25/new-octopus-deploy-step/
---
My new pull request was [recently merged](https://github.com/OctopusDeploy/Library/pull/448) to the [Octopus Deploy](https://octopus.com/) Library.

Content Delivery Network (CDN) cache needs to be refreshed after deploying a Web UI project. I developed a custom step which runs a PowerShell script on Octopus, a well-known deployment tool in .NET eco-system, to trigger purge method of the CDN server which is [Medianova CDN](http://www.medianova.com/servisler/statik-icerik-hizlandirma/) and added it to community step library.

Now, you can see the details of [Medianova - Purge Step](https://library.octopus.com/step-templates/dce70842-466e-4ae7-acd4-9aa18bfac065) on Octopus Deploy Library and install it from the community step templates page of the Octopus dashboard.

![MedianovaStepTemplate]({{site.baseurl}}/img/MedianovaStepTemplate.PNG)

PS: We should add the version parameter to the static files. This process will not renew the browser cache.
