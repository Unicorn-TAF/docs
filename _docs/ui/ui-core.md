---
title: UI Core
category: UI
weight: 5
order: 1
---

Unicorn has Core UI library in its ecosystem which provides generic mechanism and utilities for platform dependent UI testing modules and contains common utilities and UI assertions.

## Common interfaces

 - **ISearchContext**: Represent UI search context to search child elements from.
 - **IDriver**: Interface for specific UI platform driver.
 - **IControl**: Interface for specific UI controls.

## Search mechanism

Elements search is built over `BaseSearchContext` class which provides methods to search elements by different criteria. `BaseSearchContext` is used as a base class for all search contexts.

There are two ways to search elements: using search context direct methods calls or using page object approach.

### Search context methods
 - **Find\<T\>**: Finds control of specified type by specified locator during implicitly wait timeout.
 - **Find**: Finds and returns generic IControl by specified locator during implicitly wait timeout.
 - **FindList\<T\>**: Finds list of controls of specified type by specified locator during implicitly wait timeout.
 - **TryGetChild**: Immediately and safely tries to get control by specified locator with optional `out` control parameter.

Control locator is presented by `Unicorn.UI.Core.Driver.ByLocator` which consists of search method and search query. Supported locators are: 
 - Id
 - Name
 - Class
 - WebTag (web only)
 - WebXpath (web only)
 - WebCss (web only)

Examples:

```csharp
var searchContext; // let it be some search context

Button button = searchContext.Find<Button>(ByLocator.Id("buttonId"));
TextBox input = searchContext.Find<TextBox>(ByLocator.Name("inputName"));
List<Checkbox> list = searchContext.FindList<CheckBox>(ByLocator.Class("checkBoxClass"));

bool windowFound = searchContext.TryGetChild(ByLocator.Id("childId"), out Window childControl);
```

### PageObject approach

PageObject approach is a way to organize your tests in a more structured and readable way. It involves creating a class that represents a page/screen/window etc. and then using that class to interact with the page elements. This approach can help you to avoid duplicating code and make your tests easier to maintain. In Unicorn each control could be a PageObject for it's child controls what increases UI code re-use rate.

```csharp
public class PageObjectExample
{
    // Page object controls could be class properties (should have a setter).
    [Name("Page title")]
    [Find(Using.WebCss, "section[style *= block] h1.heading-separator")]
    public WebControl MainTitle { get; set; }

    // Or class fields
    // Each page object control could have readable name specified through NameAttribute.
    // This generate human friendly ToString for the control and makes reports and logs more readable.
    [Name("Page footer")]
    [Find(Using.Id, "footer")]
    private readonly WebControl _footer;
    
    // Besides generic FindAttribute there are simlified shortcuts for locators
    [Name("Page header")]
    [ById("hero")]
    public WebControl Header { get; set; }

    // If a control has the same locator across all the places, then the locator and the name could be specified for the control type using the same FindAttribute and NameAttribute.
    // In such case the control also will be initialized with page object.
    public ModalWindow Modal { get; set; }
}

// Typified or complex UI control should inherit WebControl
[Name("Modal window")]
[Find(Using.Id, "modalWindow")]
public class ModalWindow : WebControl
{
    [Name("Close button"), Find(Using.WebCss, "a[onclick]")]
    private WebControl closeButton;

    [Name("Text content"), ById("caption")]
    public WebControl TextContent { get; set; }
}
```

Available locator attributes shortcuts:
 - `[ById]`
 - `[ByClass]`
 - `[ByName]`

## Explicit waits
There are built-in explicit waiters for a control to be in desired state (visible, enabled etc.). These waiters are available as extension methods for `IControl` interface (`Unicorn.UI.Core.Synchronization.ControlWaits`). Waiters based on specific conditions are available in `Unicorn.UI.Core.Synchronization.Conditions.Until` class. Next conditions are available from the box as extensions:
 - Until.Visible
 - Until.NotVisible
 - Until.Enabled
 - Until.Disabled
 - Until.AttributeContains
 - Until.AttributeDoesNotContain
 - Until.AttributeHasValue

You can also create your own conditions. Here is an example of custom condition extension:

```csharp
public static class CustomConditions
{
    // condition to wait until control has some expected text
    public static TTarget HasText<TTarget>(this TTarget element, string expectedText) 
        where TTarget : class, IControl
    {
        if (!element.Text.Equals(expectedText))
        {
            return null;
        }

        return element;
    }
}
```

Here are examples of usage of explicit waits:

```csharp
using Unicorn.UI.Core.Synchronization;
using Unicorn.UI.Core.Synchronization.Conditions;

Window window; // assume the control is already initialized

// wait for 10 seconds until window is visible
window.Wait(Until.Visible, TimeSpan.FromSeconds(10));

// wait for 1 minute until window is disabled with custom error message
window.Wait(Until.Disabled, TimeSpan.FromMinutes(1), 
    "Window is not disabled after 1 minute");

// chain waits
window.Wait(Until.AttributeContains, "value", "someValue")
    .Wait(CustomConditions.HasText, "expectedText", TimeSpan.FromSeconds(10));
```

### Loading indicators handling
Many web applications use loading indicators to indicate that some operation is in progress. Handling these indicators is an important part of UI automation. To correctly handle such indicators it's not enough to wait for the indicator to disappear, often there is some delay before the indicator appears and it's important to wait for it to appear as well. This ensures that the operation has completed and the UI is in a stable state.  
To handle loading indicators Unicorn has built-in `LoaderHandler` class. This class allows you to wait for a loading indicator to appear and then disappear before proceeding with further actions. 

> Let's suppose we have:  
> - loading indicator with the ID **"loading-indicator"**
> - **popUpWindow** - is a control that represents the pop-up window and where we need to wait for the loading indicator to appear and disappear

The code snippet below demonstrates how to use the `LoaderHandler` class in this case:
```csharp
new LoaderHandler(
        () => TitleDropdown.TryGetChild(ByLocator.Id("loading-indicator")),
        () => !TitleDropdown.TryGetChild(ByLocator.Id("loading-indicator")))
    .WaitFor(TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10));
```

This code safely waits for 2 seconds until element with ID "loading-indicator" appears and then waits for 10 seconds until it disappears. If the loading indicator does not appear within 2 seconds the method will not fail (sometimes catching appearance is tricky and it could be too fast to be detected). If the loading indicator does not disappear within 10 seconds, the `WaitFor` method will throw an exception.

It's possible also to wait with custom polling interval, just use corresponding `WaitFor` overload:

```csharp
new LoaderHandler(
        () => TitleDropdown.TryGetChild(ByLocator.Id("loading-indicator")),
        () => !TitleDropdown.TryGetChild(ByLocator.Id("loading-indicator")))
    .WaitFor(TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(30), TimeSpan.FromSeconds(1));
);
```