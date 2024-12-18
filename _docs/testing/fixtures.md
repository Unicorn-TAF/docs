---
title: Fixtures
category: Testing
weight: 3
order: 3
---

Fixtures are special testing methods which help provide a consistent environment for tests to run in. This can include things like setting up a database, creating test data, or configuring application settings. Fixtures can be used to ensure that tests are not affected by changes in the environment, and that they are repeatable and reliable.

Unicorn has the following types of fixtures: assembly fixtures, suite fixtures.

## Assembly fixtures

Assembly fixtures are used to set up and tear down the environment for whole test assembly. They are run before and after all tests in the assembly. Assembly fixtures are defined using the `[RunInitialize]` and `[RunFinalize]` attributes. The methods should be static and should not take any parameters and be located in a class marked with the `[TestAssembly]` attribute. Here is an example:

```csharp
using Unicorn.Taf.Core.Testing.Attributes;

// Actions performed before and/or after all tests execution.
[TestAssembly]
public class TestsAssembly
{
    // Actions before all tests execution.
    // The method should be static.
    [RunInitialize]
    public static void InitRun()
    {
        // Initialize reporter, set logger etc
    }

    // Actions after all tests execution.
    // The method should be static.
    [RunFinalize]
    public static void FinalizeRun()
    {
        // Final cleanup, etc.
    }
}
```

If RunInitialize is failed the test run will be skipped.

## Suite fixtures

Suite fixtures are used to set up and tear down the environment for a test suite. They are run before and after all tests in the suite. Suite fixtures are defined using the `[BeforeSuite]` and `[AfterSuite]` attributes. The methods should not take any parameters and be located in a suite class or base class. In case of parameterized test suite fixtures are run for each suite data set entry. Here is an example:

```csharp
using Unicorn.Taf.Core.Testing.Attributes;

[Suite]
public class WebSuite
{
    // Actions executed before all tests in current suite.
    [BeforeSuite]
    public void ClassInit()
    {
        // Initialize web driver, etc.

    }

    // Actions executed after all tests in current suite.
    [AfterSuite]
    public void ClassTearDown()
    {
        // close browser, etc.
    }
}
```

If `BeforeSuite` is failed the suite will be skipped.

## Test fixtures

Test fixtures are used to set up and tear down the environment for a test. They are run before and after each test. Test fixtures are defined using the `[BeforeTest]` and `[AfterTest]` attributes. The methods should not take any parameters and be located in a test class or base class. In case of parameterized test fixtures are run for each test data set entry. 

It's possible to configure `[AfterTest]` behavior:
 - whether to run `[AfterTest]` if the test is failed or not. 
 - whether to skip all following tests in suite if `[AfterTest]` is failed or not. 

Here is an example:

```csharp
using Unicorn.Taf.Core.Testing.Attributes;

[Suite]
public class WebSuite
{
    // Actions executed before each tests in current suite.
    [BeforeTest]
    public void TestInit()
    {
        // Initialize web driver, etc.
    }

    // Actions executed after each tests in current suite.
    [AfterTest]
    public void TestCleanup()
    {
        // Close browser, etc.
    }

    // Tear down will not be executed if the test is failed.
    [AfterTest(true)]
    public void CleanupSkippedOnTestFail()
    {
        // Some code...
    }

    // Tear down will not be executed if the test is failed, 
    // but when it's failed itself all next tests in suite will be skipped.
    [AfterTest(true, true)]
    public void CleanupSkippedOnTestFailAndAffectingNextTests()
    {
        // Some code...
    }
}
```

