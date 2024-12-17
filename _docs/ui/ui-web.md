---
title: Web UI
category: UI
order: 3
---

[Unicorn.UI.Web](https://www.nuget.org/packages/Unicorn.UI.Web) is a library that provides a set of tools for testing web user interfaces. It is built on top of [Selenium](https://www.selenium.dev) and provides a higher-level API for interacting with browser, web pages and web elements.

The package contains:
* Web Driver implementation
* Typified controls implementations
* PageObject implementation
* Abstract WebSite and pages pool

## PageObject

Page object supports inheritance, so all derived classes initialize controls described in base class also. Any page should be derived from `WebPage`. Page properties like relative url and title could be specified using `[PageInfo]` attribute. Url property is implicitly used to navigate to the page from `WebSite` instance. Title property is implicitly used in page `Opened` property indicating that page is opened successfully.

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