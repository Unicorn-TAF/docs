---
title: Web UI
category: UI
weight: 5
order: 3
---

[Unicorn.UI.Web](https://www.nuget.org/packages/Unicorn.UI.Web) is a library that provides a set of tools for testing web user interfaces. It is built on top of [Selenium](https://www.selenium.dev) and provides a higher-level API for interacting with browser, web pages and web elements.

The package contains:
* Web Driver implementation
* Typified controls implementations
* PageObject implementation
* Abstract WebSite and pages pool

## PageObject

Page object supports inheritance, so all derived classes initialize controls described in base class also. Any page should be derived from `WebPage`. Page properties like relative URL and title could be specified using `[PageInfo]` attribute. URL property is implicitly used to navigate to the page from `WebSite` instance. Title property is implicitly used in page `Opened` property indicating that page is opened successfully.

```csharp
[PageInfo("test-ui-apps.html", "Test UI apps | Unicorn.TAF")]
public class ExamplePage : WebPage
{
    // Creating page instance with <see cref="WebDriver"/> context.
    public ExamplePage(WebDriver driver) : base(driver)
    {
    }

    // Page object controls could either class properties or class fields (properties should have a setter).
    [Name("Page title")]
    [Find(Using.WebCss, "section[style *= block] h1.heading-separator")]
    public WebControl MainTitle { get; set; }

    // Each page object control could have readable name specified through NameAttribute.
    // This generate human friendly ToString for the control and makes reports and logs more readable.
    // Besides generic FindAttribute there are simlified shortcuts for locators:
    // ByIdAttribute, ByClassAttribute, ByNameAttribute
    [Name("Page header")]
    [ById("hero")]
    public WebControl Header { get; set; }

    // If a control has the same locator across all the places, then the locator and the name could be 
    // specified for the control type using the same FindAttribute and NameAttribute.
    // In such case the control also will be initialized with page object.
    public ModalWindow Modal { get; set; }

    public void WaitForLoading() =>
        Header.Wait(Until.Visible, Timeouts.PageLoadTimeout);
}
```

## Custom website implementation
The base website gives access to underlying WebDriver instance, provides with pages cache  mechanism and eases pages navigation. Some site specific actions and helpers could be placed here. The website should inherit `WebSite`.

```csharp
public class TestWebsite : WebSite
{
    // Website creation based on existing explicitly created WebDriver instance.
    // Should call base constructor.
    public TestWebsite(WebDriver driver, string siteUrl) : base(driver, siteUrl)
    {
    }

    // Website creation based on retrieved type of browser (WebDriver in this case is created automatically).
    // Should call base constructor.
    public TestWebsite(BrowserType browser, string siteUrl) : base(browser, siteUrl)
    {
    }
}
```

## Page pool

Page pool mechanism helps to avoid uncontrolled web pages objects creation each time. For a single WebSite instance only page pool allows to create only one page instance.

```csharp
TestWebsite website = new TestWebsite(....);

// All you need to create a new website page is to call 
ExamplePage page = website.GetPage<ExamplePage>();
// if the page was already created for current webdriver, existing instance will be returned
// otherwise new instance will be created

// To navigate directly to the page by it's url it's enough to call
ExamplePage page = website.NavigateTo<ExamplePage>();
```

## Dynamic controls
There are cases when one needs to implement some complex UI element such as Dropdown or DataGrid which is different from all other implementations and can't be implemented using standard controls. For such cases, it's possible to define a _dynamic_ control without creating a separate implementation. There are 3 complex controls which are implemented as dynamic controls: `DynamicDropdown`, `DynamicDialog` and `DynamicDataGrid`.

Example below is for DataGrid, all you need is just within PageObject class to define the control as:

```csharp
// Find top level control
[ById("dg-demo-static-data")]  
// And define sub-elements
[DefineGrid(GridElement.Header, Using.WebTag, "th")]
[DefineGrid(GridElement.Row, Using.WebCss, "tbody > tr")]
[DefineGrid(GridElement.Cell, Using.WebCss, "td")]
public DynamicDataGrid DataGrid { get; set; }
```

The whole logic of interaction with certain UI element is baked in corresponding dynamic element and there is no need to implement it, just to define necessary sub-elements.

To define `DynamicDataGrid` sub-elements `[DefineGrid]` is used. It's possible to define _Header_, _Row_, _Cell_ and _Loader_ elements.

To define `DynamicDropdown` sub-elements `[DefineDropdown]` is used. It's possible to define _ValueInput_, _ExpandCollapse_ button, _OptionsFrame_, dropdown _Option_ and _Loader_ elements.

To define `DynamicDialog` sub-elements `[DefineDialog]` is used. It's possible to define _Title_, _Content_, _Accept_, _Decline_ and _Close_ buttons, _Loader_ elements.