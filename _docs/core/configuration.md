---
title: Configuration
category: Core
order: 1
---

Unicorn could be configured in two ways: using unicorn configuration file or from code.

Unicorn configuration file is a simple JSON file. The file could be loaded by TestAdapter based on _.runsettings_ file or from code by calling `Config.FillFromFile`.

## Properties

All properties are optional (in case of property absence default value is used)

### testsDependency
> **In code:** `Config.DependentTests`

Specifies how to deal with dependent tests in case when main test was failed. Available options:
 - Skip
 - DoNotRun
 - **Run** _(default)_

### testsOrder
> **In code:** `Config.TestsExecutionOrder`

Specifies in which order to run tests within a suite. Available options:
 - Alphabetical
 - Random
 - **Declaration** _(default)_

### parallel
> **In code:** `Config.ParallelBy`

Specifies how to parallel tests execution. Available options:
 - Suite
 - **None** _(default)_

### threads
> **In code:** `Config.Threads`

Specifies how many threads are used to run tests in parallel.  
_Default:_ **1**

### testTimeout
> **In code:** `Config.TestTimeout`

Specifies timeout for test and suite method execution in minutes.  
_Default:_ **15**

### suiteTimeout
> **In code:** `Config.SuiteTimeout`

Specifies timeout for test suite execution in minutes.  
_Default:_ **40**

### tags
> **In code:** `Config.RunTags`

List of suites tags to be run.  
_Default:_ **empty** (all)

### categories
> **In code:** `Config.RunCategories`

List of test categories to be run.  
_Default:_ **empty** (all)

### tests
> **In code:** `Config.RunTests`

List of tests masks to be run.  
_Default:_ **empty** (all)

> if `tests` is specified, `tags` and `categories` properties are ignored.

### userDefined
Parent for user defined properties. Any number of custom properties as key-value pair could be specified. Value of specific user defined setting could be retrieved by calling `Config.GetUserDefinedSetting`.

## Example of properties file
```json
{
    "testsDependency": "Skip",
    "testsOrder": "Random",
    "parallel": "Suite",
    "threads": 2,
    "testTimeout": 15,
    "suiteTimeout": 60,
    "tags": [ "Tag1", "Tag2" ],
    "categories": [ "Smoke" ],
    "tests": [ ],
    "userDefined": {
        "customProperty1": "value1",
        "customProperty2": "value2"
    }
}
```

> If the config was loaded from inside of test run (for example in [TestAssembly] fixture) `tags`, `categories` and `tests` properties are ignored because test run scope is already defined and could not be changed from the test run.

## Reset

It's possible to reset config by calling `Config.Reset()` method. This will remove all custom properties and set all properties to their default values.
