---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: 'C# 7.0 New Features in One File'
image: /img/c-sharp-logo.png
tr: /2017/05/02/c-sharp-7-ile-gelen-yeni-yetenekler/
tags:
  - 'C#'
---
C# now has some new features with the release of Visual Studio 2017. Those features focused on code simplification, performance and data consumption. I prepared a simple console application to show them in one file.

### Out variables
`out variables` lets us to declare variables inline. I you don't want to declare a variable for any of out variable you can use `_` sign.

### Patterns "is"
_Constant_, _Type_ or _Var_ patterns can be used in if statements and you can declare new inline variables as in `out variables`.

### Switch statements
Switch has biggest improvements. You can switch, not just primitive types, any type you want. Patterns can be used in case clauses which can have additional conditions with `when` keyword.
<!--more-->
### Tuples
_Tuples_ are really great but it was a backpain to use `System.Tuple<...>`. Now, we just add `System.ValueTuple` reference and it comes with _tuple types_ to return and _tuple literals_ to declare.

If you didn't give any name to variables, you can reach to those variables as Item1, Item2 etc.

### Local Functions
You can now declare helper functions inside a function for example recursive functions. This helps us to tidy our code.

### Literal improvements
You can use digit seperator `_` to make the code more readable.

### Ref returns and locals
Now _ref_ modifier return more than the value itself, the reference which shows the storage location in an array.

You can see all of the new features down below;

<script src="https://gist.github.com/olcay/e8954ab45ba7b2a0bcd842c4f76c668e.js"></script>

PS: You need to install free [Visual Studio 2017 Community](https://www.visualstudio.com/downloads/) to use those features.

Source: [New Features in C# 7.0](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/)

