---
title: Test steps
category: Testing
weight: 3
order: 4
---

Test steps are special methods which allow to fire framework events such as step start and step finish. They are used by supported reporting agents to report test steps. It's also possible to subscribe to step events and execute custom user action.

To make the functionality working you need to add `Unicorn.Taf.StepsInjection` dependency to your project. Then you need to add `[StepsClass]` attribute to classes containing test steps. Test Steps methods should be marked with `[Step]` attribute.


## Step class example
```csharp
using Unicorn.Taf.Core.Steps.Attributes;
using Unicorn.Taf.StepsInjection;

// Mark steps class
[StepsClass]
public class MySteps
{
    // Step without parameters
    [Step("My custom step")]
    public void MyStep()
    {
        
    }

    // Step with parameters
    [Step("My custom step with param1 {0} and param2 {1}")]
    public void MyStep(string param1, int param2)
    {
        
    }
}
```

## Run custom action as a step

In case when you need to execute a custom action as a step without creating a separate step method, you can use the `Step` method from the `Steps` class.
It will act as a wrapper around the custom action and execute it as a step with firing step start and step finish events.

```csharp
using static Unicorn.Taf.StepsInjection.Steps;

// Execute step as Action without sepearate step method
Step("Execute step as an action", () => /* Some code to call */);
```
