---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: Code in Stranger Things 2
image: /img/hawkinsLabSystemsControl.png
tags:
  - movie
  - basic
---
If you want to break a software's security pass in early 1980's, you need to code in BASIC like in the series [Deutchland 83](http://olcay.dev/2017/05/04/deutschland-83-encoded-floppy-disc/)

Stranger Things 2 is in 1984 as well so once again we are dealing with some BASIC code.

[![hawkinsLabSystemsHack.png]({{site.baseurl}}/img/hawkinsLabSystemsHack.png)]({{site.baseurl}}/img/hawkinsLabSystemsHack.png)

As you can clearly see there are nested 4 loops to find 4 digit numbers of the password but interesting part is that there is a misterious function called `checkPasswordMatch` which gets an integer as a parameter and returns a boolean result. Actually the important part is in that function, the rest that we see on the screen just tries some brute-force attack.

<!--more-->
_Spoiler alert!_

I am glad to see Sam from LOTR hacking some computer.

