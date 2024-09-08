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

We will discuss about frequently used ones to run test scenarious properly. You would find comparisons of [NUnit](https://github.com/nunit/docs/wiki/Attributes), [MSTest](https://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.unittesting.classinitializeattribute.aspx), [xUnit.net](https://xunit.net/) and [JUnit](https://junit.org) testing frameworks down below.

You could find very comprehensive tutorials on [Guru99](https://www.guru99.com/junit-annotations-api.html) website.

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

Let's build a test project to see the order of the attributes preciesly.

Create a new Class Library project and a method to test like the one below.

{% highlight csharp linenos %}
namespace OtomatikMuhendis.TestSample
{
  public class DivideClass
  {
    public static int DivideMethod(int numerator, int denominator)
    {
      return (numerator / denominator);
    }
  }
}
{% endhighlight %}

Add an Unit Test Project to the Solution and add a reference of the class library project.

Replace the unit test class with the following example.

{% highlight csharp linenos %}
using Microsoft.VisualStudio.TestTools.UnitTesting;
using OtomatikMuhendis.TestSample;
using System.Diagnostics;

namespace OtomatikMuhendis.UnitTest
{
  [TestClass]
  public sealed class DivideClassTest
  {
    [AssemblyInitialize]
    public static void AssemblyInit(TestContext context)
    {
      Debug.WriteLine("AssemblyInit " + context.TestName);
    }

    [ClassInitialize]
    public static void ClassInit(TestContext context)
    {
      Debug.WriteLine("ClassInit " + context.TestName);
    }

    [TestInitialize]
    public void Initialize()
    {
      Debug.WriteLine("TestMethodInit");
    }

    [TestMethod]
    [TestCategory("MathLibTests")]
    public void DivideMethod_DivideByOne_ResultIsEqual()
    {
      //Arrenge
      var numerator = 6;
      var denominator = 1;

      //Act
      var result = DivideClass.DivideMethod(numerator, denominator);

      //Assert
      Assert.AreEqual(numerator, result);

      Debug.WriteLine("TestMethod_DivideMethod_DivideByOne_ResultIsEqual");

    }

    [TestMethod]
    [TestCategory("MathLibTests")]
    public void DivideMethod_DivideByTwo_ResultIsHalf()
    {
      //Arrenge
      var numerator = 6;
      var denominator = 2;

      //Act
      var result = DivideClass.DivideMethod(numerator, denominator);

      //Assert
      Assert.AreEqual(numerator, result * denominator);

      Debug.WriteLine("TestMethod_DivideMethod_DivideByTwo_ResultIsHalf");

    }

    [TestMethod]
    [TestCategory("MathLibTests")]
    [ExpectedException(typeof(System.DivideByZeroException))]
    [Ignore]
    public void DivideMethod_DivideByZero_ThrowsDivideByZeroException()
    {
      //Arrenge
      var numerator = 6;
      var denominator = 0;

      //Act
      var result = DivideClass.DivideMethod(numerator, denominator);

      //Assert
      Assert.AreEqual(numerator, result * denominator);

      Debug.WriteLine("TestMethod_DivideMethod_DivideByZero_ThrowsDivideByZeroException");

    }

    [TestCleanup]
    public void Cleanup()
    {
      Debug.WriteLine("TestMethodCleanup");
    }

    [ClassCleanup]
    public static void ClassCleanup()
    {
      Debug.WriteLine("ClassCleanup");
    }

    [AssemblyCleanup]
    public static void AssemblyCleanup()
    {
      Debug.WriteLine("AssemblyCleanup");
    }
  }
}
{% endhighlight %}

<span class="responsiveImg">
[![testingProject.png]({{site.baseurl}}/img/testingProject.png)]({{site.baseurl}}/img/testingProject.png)
</span>
*Project will be like this*

Output of our test run;

    AssemblyInit DivideMethod_DivideByOne_ResultIsEqual
    ClassInit DivideMethod_DivideByOne_ResultIsEqual
    TestMethodInit
    TestMethod_DivideMethod_DivideByOne_ResultIsEqual
    TestMethodCleanup
    TestMethodInit
    TestMethod_DivideMethod_DivideByTwo_ResultIsHalf
    TestMethodCleanup
    ClassCleanup
    AssemblyCleanup
