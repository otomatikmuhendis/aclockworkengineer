---
author: Olcay Bayram
layout: post
title: Attributes and Annotations of Testing Frameworks
tr: /2016/10/09/test-framework-attribute-annotation/
categories: Testing
tags: 
  - test
  - nunit
  - xunit
  - mstest
  - junit
published: true
subtitle: Frequently used attribute and annotations in various testing frameworks 
---
It is called attributes in the .NET environment and annotations in Java. We use them to declare information about methods, types, properties and so on.

We will discuss about frequently used ones to run test scenarious properly. You would find comparisons of [NUnit](https://github.com/nunit/docs/wiki/Attributes), [MSTest](https://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.unittesting.classinitializeattribute(v=vs.140).aspx), [xUnit.net](https://xunit.github.io/) and [JUnit](https://www.tutorialspoint.com/junit/junit_using_assertion.htm) testing frameworks down below.

|NUnit|MSTest|xUnit.net|JUnit|Description|
|---|---|---|---|---|
|[TestFixture]|[TestClass]|-|-|Indicates that the class has test methods.|
|[Test]|[TestMethod]|[Fact]|@Test|Marks a test case.|
|[OneTimeSetUp]|[ClassInitialize]|IClassFixture<T>|@BeforeClass|The one time triggered method before test cases start.|
|[OneTimeTearDown]|[ClassCleanup]|IClassFixture<T>|@AfterClass|The one time triggered method after test cases end.|
|[SetUp]|[TestInitialize]|Constructor|@Before|Triggered before every test case.|
|[TearDown]|[TestCleanup]|IDisposable.Dispose|@After|Triggered after every test case.|
|[Ignore]|[Ignore]|[Fact(Skip="reason")]|@Ignore|Ignores the test case.|
|[Category("")]|[TestCategory("")]|[Trait("Category", "")]|@Category(*.class)|Categorizes the test cases or classes.|

<!--more-->

There is a little difference between them except that the xUnit framework. xUnit prefers inheritance for the ones that it doesn't want to be used very often.

Category separation is a best practice on testing because you could see your test cases by its category on your test runner tool or run them in a Continuous Integration application seperately.

If you don't want to run a test temporarily you could use ignore attribute. Your test runner tool will skip that test and show it with the provided message.

<span class="responsiveImg">
[![testingIgnore.png]({{site.baseurl}}/img/testingIgnore.png)]({{site.baseurl}}/img/testingIgnore.png)
</span>
*Display on Test Explorer*

<span class="responsiveImg">
[![testingIgnoreJenkins.png]({{site.baseurl}}/img/testingIgnoreJenkins.png)]({{site.baseurl}}/img/testingIgnoreJenkins.png)
</span>
*Display on Jenkins CI Test Coverage Report*