---
title: Waiter
category: "Helpers & utilities"
weight: 7
order: 4
---

## Overview
The `DefaultWait` class provides a basic implementation for waiting until a specified boolean condition is met. It supports configurable timeout periods and polling intervals, with both strict and safe waiting modes.

Namespace: `Unicorn.Taf.Core.Utility.Synchronization`

## Behavior

### Waiting Process
1. Validates that the condition is not `null`
2. Logs the start of wait operation with condition name, timeout, and polling interval
3. Starts the expiration timer
4. Enters polling loop:
    - Invokes the condition function
    - If condition returns `true` → wait successful, returns `true`
    - If exception occurs → checks if it should be ignored
    - Checks if timeout has expired
    - Sleeps for the polling interval

5. Continues until condition met or timeout expires

### Exception Handling
 - Exceptions thrown by the condition can be ignored if added to IgnoredExceptions list
 - If an exception type is not in the ignored list, it is thrown immediately
 - Ignored exceptions are stored as `lastException` and included in timeout exception if one occurs

### Timeout Behavior
 - `Until()` method: Throws `TimeoutException` when timeout expires
 - `SafelyUntil()` method: Logs a warning and returns `false` when timeout expires


## Usage Examples

### Basic Wait with Default Settings
```csharp
var wait = new DefaultWait();
wait.Until(() => IsElementVisible());
```

### Custom Timeout Only
```csharp
var wait = new DefaultWait(TimeSpan.FromSeconds(30));
wait.Until(() => FileExists("data.txt"));
```

### Custom Timeout and Polling Interval
```csharp
var wait = new DefaultWait(
    TimeSpan.FromSeconds(45), 
    TimeSpan.FromMilliseconds(500)
);
wait.Until(() => ServiceIsAvailable());
```

### Safe Waiting (No Exceptions)
```csharp
var wait = new DefaultWait(TimeSpan.FromSeconds(10));

if (wait.SafelyUntil(() => DatabaseIsReady()))
{
    Console.WriteLine("Database is ready");
}
else
{
    Console.WriteLine("Database readiness check timed out");
}
```

### Configuring Ignored Exceptions
```csharp
var wait = new DefaultWait(TimeSpan.FromSeconds(30));

// Inherited from BaseWait
wait.IgnoreExceptionTypes(typeof(WebDriverException), typeof(ElementNotFoundException));

wait.Until(() => 
{
    // Will retry on WebDriverException and ElementNotFoundException
    return FindElement(By.Id("submit"));
});
