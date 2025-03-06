---
title: Assertions on UI elements
category: UI
weight: 5
order: 2
---

[Unicorn.UI.Core](https://www.nuget.org/packages/Unicorn.UI.Core) has collection of UI checking matchers compatible with [core assertions mechanism](../../testing/assertions). The matchers can be used to validate presence, state or properties of UI elements and does not depend on the underlying UI framework. Here is an example of how to use these assertions:

```csharp
Assert.That(uiElement, UI.Control.HasText("Some expected text"));

Assert.That(uiElement, Is.Not(UI.Control.HasAttributeContains("class", "some class")));

// For this check UI element should implement ICheckbox
Assert.That(checkboxElement, Is.Not(UI.Checkbox.Checked()));

// For this check UI element should implement IDropdown
Assert.That(dropdownElement, UI.Dropdown.HasSelectedValue("expected value"));

// For this check UI element should implement ITextInput
Assert.That(dropdownElement, UI.TextInput.HasValue("expected value"));

// For this check UI element should implement IDataGrid
Assert.That(dataGridElement, UI.DataGrid.HasColumn("some_column"));

// For this check UI element should implement IWindow
Assert.That(windowElement, UI.Window.HasTitle("some title"));

// For this check UI element should implement IHasValueValidation
Assert.That(uiElement, UI.Control.HasValidationError());

// and many others
```

Full list of build-in UI matchers is available as children of `Unicorn.UI.Core.Matchers.UI`