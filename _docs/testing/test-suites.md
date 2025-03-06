---
title: Test suites
category: Testing
weight: 3
order: 1
---

Test suite is a class-container of tests. It is used to group tests together and to provide metadata about the tests. Test suites can be used to run tests in parallel, to skip tests, and to provide a summary of the test results.
Test suites are defined using the `[Suite]` attribute. The `[Suite]` attribute takes a string parameter that specifies the name of the test suite. If suite name is not specified then type name is used as test suite name.

## Suite metadata
It's possible to specify test suite metadata using the `[Metadata]` attribute. The `[Metadata]` attribute takes two parameters: the name of the metadata and the value of the metadata. Metadata can be used to provide additional information about the test suite, such as the description of the test suite, the site link, etc.

## Tags
It's possible to specify tags for test suite using the `[Tag]` attribute. The `[Tag]` attribute takes a string parameter that specifies the name of the tag. Tags can be used to filter tests, for example, to run only tests that are tagged with a specific platform or application.
Tags can be specified multiple times for a single test suite.


## Example of test suite
Here is an example of a test suite that contains a single test method:

```csharp
[Suite("Hello World desktop app")]
[Tag(Platforms.Win), Tag(Apps.HelloWorld)]
[Metadata("Description", "Example of test suite. Сhecks functionality of hello world app")]
[Metadata("Site link", "https://unicorn-taf.github.io/test-ui-apps.html")]
public class HelloWorldDesktopSuite
{
    [Test]
    public void TestMethod()
    {
        //... test code
    }

}
```

## Parameterized test suites

It's possible to parameterize test suites using the `[Parameterized]` attribute. The `[Parameterized]` attribute tells unicorn engine that current class should be instantiated via parameterized constructor using parameters returned by static method marked with `[SuiteData]` attribute. This allows to run the same test suite with different parameters.

Here is an example of parameterized test suite:

```csharp
[Parameterized, Suite]
public class SampleAppDefaultsWebSuite
{
    public SampleAppDefaultsWebSuite(BrowserType browser)
    {
        _browser = browser;
    }

    [SuiteData]
    public static List<DataSet> GetBrowsers() =>
        new List<DataSet>
        {
            new DataSet("Google Chrome", BrowserType.Chrome),
            new DataSet("Edge", BrowserType.Edge),
        };
}
```
Here `GetBrowsers` is a data provider for the test suite. The method should be static and return List of `DataSet` objects. Each `DataSet` object represents a set of parameters that will be passed to the constructor of the test suite. The first parameter of `DataSet` is data set name (typically it is used for reporting and for generation of suite instance unique ID). The constructor of the test suite should accept parameters that match the DataSet objects except for the first one (data set name).

