---
title: Retrier
category: "Helpers & utilities"
weight: 7
order: 1
---

## Overview
The `Retrier` class provides a utility for executing actions with retry capabilities. It allows filtering by specific exception types and executing pre-retry actions.

Namespace: `Unicorn.Taf.Core.Utility`

## Behavior

### Retry Logic
1. The retrier executes the provided action/function

2. If an exception occurs:
 - It checks if the exception type matches the specified exceptions (if any)
 - If the number of attempts hasn't been exceeded and exception is retryable, it:
   - Logs a warning message
   - Executes the pre-retry action (if specified)
   - Retries the action

3. If all retry attempts fail or the exception is not retryable, the original exception is thrown

### Exception Handling
- If no exceptions are specified using OnExceptions(), all exceptions will trigger a retry
- When exceptions are specified, only those exact types or their subclasses will trigger a retry
- The retrier unwraps TargetInvocationException to access the actual inner exception
- If the inner exception is null, the outer exception is thrown


## Usage Examples

### Basic Usage - Single Retry
```csharp
var retrier = new Retrier();
retrier.Execute(() => SomeMethodThatMayFail());
```

### Custom Retry Count
```csharp
var retrier = new Retrier(3); // Retry up to 3 times
retrier.Execute(() => SomeMethodThatMayFail());
```

### Filtering Specific Exceptions
```csharp
var retrier = new Retrier(3)
    .OnExceptions(typeof(TimeoutException), typeof(IOException));

retrier.Execute(() => 
{
    // Will only retry on TimeoutException or IOException
    PerformNetworkOperation();
});
```

### Adding Pre-Retry Action
```csharp
var retrier = new Retrier(3)
    .DoBeforeRetry(() => 
    {
        // Reconnect or cleanup before retry
        ReinitializeConnection();
        ClearCache();
    });

retrier.Execute(() => PerformSensitiveOperation());
```

### Working with Return Values
```csharp
var retrier = new Retrier(2)
    .OnExceptions(typeof(InvalidOperationException));

string result = retrier.Execute(() => 
{
    return FetchDataFromService();
});
```

### Fluent Configuration
```csharp
new Retrier(5)
    .OnExceptions(typeof(SqlException), typeof(TimeoutException))
    .DoBeforeRetry(() => LogRetryAttempt())
    .Execute(() => DatabaseQuery());
