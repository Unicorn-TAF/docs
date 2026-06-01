---
title: TAF events
category: Core
weight: 2
order: 3
---

## Overview
The `TafEvents` class serves as the entry point for framework events in the Unicorn TAF. It provides a comprehensive event system for tracking test execution lifecycle, including suite-level, method-level, test-level, and step-level events.

Namespace: `Unicorn.Taf.Core`


## Events

### Suite Events
| Event | Delegate | Type | Description |
| --- | --- | --- | --- |
| OnSuiteStart | UnicornSuiteEvent | Invoked before suite execution |
| OnSuiteFinish | UnicornSuiteEvent | Invoked after suite execution |
| OnSuiteSkip | UnicornSuiteEvent | Invoked when suite is skipped |

### Suite Method Events
| Event | Delegate | Type | Description |
| --- | --- | --- | --- |
| OnSuiteMethodStart | UnicornSuiteMethodEvent | Invoked before suite method execution |
| OnSuiteMethodFinish | UnicornSuiteMethodEvent | Invoked after suite method execution |
| OnSuiteMethodPass | UnicornSuiteMethodEvent | Invoked when suite method passes |
| OnSuiteMethodFail | UnicornSuiteMethodEvent | Invoked when suite method fails |

### Test Events
| Event | Delegate | Type | Description |
| --- | --- | --- | --- |
| OnTestStart | TestEvent | Invoked before test execution |
| OnTestFinish | TestEvent | Invoked after test execution |
| OnTestPass | TestEvent | Invoked when test passes |
| OnTestFail | TestEvent | Invoked when test fails |
| OnTestSkip | TestEvent | Invoked when test is skipped |

### Step Events
| Event | Delegate | Type | Description |
| --- | --- | --- | --- |
| OnStepStart | StepEvent | Invoked before test step execution |
| OnStepFinish | StepEvent | Invoked after test step execution |
| OnStepFail | StepFailEvent | Invoked when test step fails |


## Usage Examples

### Basic Event Subscription
```csharp
public class TestEventHandlers
{
    public void RegisterHandlers()
    {
        TafEvents.OnTestStart += HandleTestStart;
        TafEvents.OnTestFinish += HandleTestFinish;
        TafEvents.OnTestPass += HandleTestPass;
        TafEvents.OnTestFail += HandleTestFail;
    }
    
    private void HandleTestStart(Test test)
    {
        Console.WriteLine($"Starting test: {test.Outcome.Title}");
    }
    
    private void HandleTestFinish(Test test)
    {
        Console.WriteLine($"Finished test: {test.Outcome.Title}, Duration: {test.Outcome.ExecutionTime}");
    }
    
    private void HandleTestPass(Test test)
    {
        Console.WriteLine($"Test passed: {test.Outcome.Title}");
    }
    
    private void HandleTestFail(Test test)
    {
        Console.WriteLine($"Test failed: {test.Outcome.Title}, Error: {test.Outcome.FailMessage}");
    }
}
```

### Suite-Level Event Handling
```csharp
public class SuiteEventLogger
{
    public SuiteEventLogger()
    {
        TafEvents.OnSuiteStart += LogSuiteStart;
        TafEvents.OnSuiteFinish += LogSuiteFinish;
        TafEvents.OnSuiteSkip += LogSuiteSkip;
    }
    
    private void LogSuiteStart(TestSuite suite)
    {
        Logger.LogInfo($"Suite {suite.Outcome.Name} started at {DateTime.Now}");
    }
    
    private void LogSuiteFinish(TestSuite suite)
    {
        Logger.LogInfo($"Suite {suite.Outcome.Name} completed. Passed: {suite.Outcome.PassedTests}, Failed: {suite.Outcome.FailedTests}");
    }
    
    private void LogSuiteSkip(TestSuite suite)
    {
        Logger.LogWarning($"Suite {suite.Outcome.Name} was skipped");
    }
}
```

### Step Event Monitoring
```csharp
public class StepMonitor
{
    public StepMonitor()
    {
        TafEvents.OnStepStart += (method, args) => 
        {
            Console.WriteLine($"Step started: {method.Name}");
            Console.WriteLine($"Arguments: {string.Join(", ", args)}");
        };
        
        TafEvents.OnStepFinish += (method, args) =>
        {
            Console.WriteLine($"Step completed: {method.Name}");
        };
        
        TafEvents.OnStepFail += (method, exception) =>
        {
            Console.WriteLine($"Step failed: {method.Name}");
            Console.WriteLine($"Error: {exception.Message}");
        };
    }
}
```

### Test Results Reporting
```csharp
public class TestReporter
{
    private int totalTests = 0;
    private int passedTests = 0;
    private int failedTests = 0;
    
    public TestReporter()
    {
        TafEvents.OnTestStart += (test) => totalTests++;
        TafEvents.OnTestPass += (test) => passedTests++;
        TafEvents.OnTestFail += (test) => failedTests++;
        TafEvents.OnSuiteFinish += GenerateReport;
    }
    
    private void GenerateReport(TestSuite suite)
    {
        Console.WriteLine($"=== Test Run Summary ===");
        Console.WriteLine($"Total: {totalTests}");
        Console.WriteLine($"Passed: {passedTests}");
        Console.WriteLine($"Failed: {failedTests}");
        Console.WriteLine($"Success Rate: {(double)passedTests / totalTests * 100}%");
    }
}
```

### Unsubscribing from Events
```csharp
public class DisposableEventHandler : IDisposable
{
    public DisposableEventHandler()
    {
        TafEvents.OnTestStart += HandleTestStart;
    }
    
    public void Dispose()
    {
        TafEvents.OnTestStart -= HandleTestStart;
    }
    
    private void HandleTestStart(Test test)
    {
        // Event handling logic
    }
}
```
