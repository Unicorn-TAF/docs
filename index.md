---
title: Welcome
---

[Unicorn](https://unicorn-taf.github.io) is all-in-one test automation framework which provides wide spectrum of functionality out of box to start efficient test automation process in short terms.
 - Free all-in-one test automation framework: unit tests, ui, api
 - Rich collection of matchers: core, ui, api
 - Integration with popular reporting tools
 - extendable UI
 - extended abilities with Unicorn.Toolbox

### Getting Started

Download example test project from [GitHub](https://github.com/unicorn-taf/Unicorn.TAF.Examples) and run it.
The project covers majority of Unicorn features across different platforms:
 - Windows
 - Web
 - Api

With ability to use and switch between supported reporters: Allure, ReportPortal, TestIt

> Feel free to contact [dobriy88@yandex.ru](mailto:dobriy88@yandex.ru) with your feedback.

Please also refer to **[MIGRATION NOTES](/migration/migration-to-taf-v4/)** in case of update to Taf.Core 4.x from older versions.

### Features

#### Unit Tests
 - Test suites and tests parameterization by test data of any complexity
 - Test suites metadata and tagging
 - Tests categories
 - Tests dependency (ability to mark and control behavior of dependent tests)
 - Ability to parallel tests execution
 - Base for self-describing steps implementation
 - Verification by Asserts + rich matchers collection from the box
 - Integration with VS Test Explorer

#### UI Support
Unicorn supports different GUI out of box including extended PageObject capabilities. Interaction with web content in browser based on Selenium WebDriver. Interaction with Windows native GUI based on Microsoft UI Automation. Interaction with web and native applications on Android devices based on Appium. Generic implementation used as a base for any specific GUI implementation. Rich collection of all-GUI matchers from the box.

 - Ready for use common controls with predefined base actions for all platforms (such as Button, TextInput, Checkbox, Dropdown, Radio etc.) with detailed logging
 - Ability to use unified controls actions across all supported platforms
 - Single set of UI controls matchers for all supported platforms
 - Unified search for any platform controls using one [Find] attribute by unified locators
 - Ability to create custom complex controls based on already existing or "Base controls" for all supported platforms
 - Ability to construct blocks from any number of controls and reuse them across different page objects

#### API Support
 - REST API client implementation
 - Ability to download files
 - Rich API matchers collection from the box