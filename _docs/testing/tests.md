---
title: Tests
category: Testing
order: 2
---

# Overview

Test is a method inside a [test suite](../test-suites) which is marked with the `[Test]` attribute. Tests could have some additional attributes, could be parameterized and could have execution order within a test suite.

## Attributes

### `[Test]`

This attribute is used to mark a method as a test. The `[Test]` attribute takes a string parameter that specifies the name of the test. If test name is not specified then method name is used as test name.

### `[Author]`

This attribute is used to specify the author of the test. The `[Author]` attribute takes a string parameter that specifies the name of the author.

### `[TestCaseId]`

This attribute is used to specify the test case id of the test. The `[TestCaseId]` attribute takes a string parameter that specifies the test case id of the test. The test case id is used to identify the test case in the test management system and internally used in reporters which support work with test cases ids.

### `[Category]`

This attribute is used to specify the category of the test. The `[Category]` attribute takes a string parameter that specifies the category of the test. The category is used to group tests together and could be used to filter tests for the test runner.


### `[Order]`

This attribute is used to specify the order of the test within a test suite. The `[Order]` attribute takes an integer parameter that specifies the order of the test. Tests with lower order values are executed first.

### `[DependsOn]`

This attribute is used to specify the test that the current test depends on. The `[DependsOn]` attribute takes a string parameter that specifies the name of the test method that the current test depends on. The test runner will execute the test that the current test depends on before executing the current test. It is possible to configure the test runner to _fail_, _skip_, or _do not run_ the test if the test it depends on fails.

### `[Bug]`

This attribute is used to specify the bug that the test is related to. The `[Bug]` attribute takes a string parameter that specifies the bug ID. This attribute can be used to track the bug in the bug tracking system and to link the test to the bug. The attribute also could be used in custom handlers to generate links to the related bug in report or to modify the test behavior based on the bug status.


## Parameterization

It's possible to parameterize the test using the `[TestData]` attribute. The `[TestData]` attribute takes a string parameter that specifies the name of the method used as a test data provider. The method should return List of `DataSet` objects. Each `DataSet` object represents a set of parameters that will be passed to the test method. The first parameter of `DataSet` is data set name (typically it is used for reporting and for generation of parameterized test unique ID). The test method should accept parameters that match the DataSet objects except for the first one (data set name).

Example
```csharp
public List<DataSet> TestParameters() =>
    new List<DataSet>
    {
        new DataSet("Correct user", new User("correct_user_name")),
        new DataSet("Invalid user", new User("invalid_user_name")),
    };

[Test("Parameterized test")]
[TestData(nameof(TestParameters))]
public void ParameterizedTest(User user)
{
    // Test logic here
}
```