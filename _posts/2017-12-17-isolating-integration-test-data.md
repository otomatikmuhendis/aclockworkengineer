---
author: Olcay Bayram
layout: post
categories: Testing
published: true
title: Isolating Integration Test Data
tr: /2017/01/09/isolating-integration-test-data/
tags:
  - NUnit
  - TransactionScope
  - isolating
---
When we write an integration test, we should leave the persistence as it was before. I will show you how to do it with NUnit easily.

After covering every corners of our code with unit tests, we can move on to integration tests that we normally test happy path scenarious. If we are using a database as a persistence then we need to run our [DML](https://en.wikipedia.org/wiki/Data_manipulation_language "Data manipulation language") commands backwards. So you should write your [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete "Create, read, update and delete") operations for **DRUC** (delete, read, update and create)  as well.

On the other hand, we can use good old [TransactionScope](https://msdn.microsoft.com/tr-tr/library/system.transactions.transactionscope(v=vs.110).aspx) in the System library. Thanks to [NUnit](https://nunit.org/) unit-testing framework, we do not need to initialize a transaciton scope every time we write a test case. We can create an attribute which is inherited ITestAction interface so it would be ran before and after every test case.

<!--more-->

In the `BeforeTest` method, it creates a new `TransactionScope`. In the `AfterTest` method, instead of calling `Complete()` function of the transaction scope to commit all the changes to the database, we call `Dispose()` to rollback the specific transaction.

```csharp
using NUnit.Framework;
using System;
using System.Transactions;

namespace OtomatikMuhendis.IntegrationTests
{
  public class Isolated : Attribute, ITestAction
  {
    private TransactionScope _transactionScope;

    public ActionTargets Targets
    {
      get { return ActionTargets.Test; }
    }

    public void BeforeTest(TestDetails testDetails)
    {
      _transactionScope = new TransactionScope();
    }

    public void AfterTest(TestDetails testDetails)
    {
      _transactionScope.Dispose();
    }
  }
}
```

We can apply the attrribute to the test cases that we want to isolate.

```csharp
[Test, Isolated]
public void Update_WhenCalled_ShouldUpdateTheGivenItem()
{ /* ... */ }
```
