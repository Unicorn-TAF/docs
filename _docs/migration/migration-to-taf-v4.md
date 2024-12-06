---
title: Migration to TAF v4
category: Migration
order: 1
---

V4 of TAF Core has some breaking changes that will require modifications to your existing codebase. This guide will help you understand the changes and how to migrate your code to TAF v4.

## General

**Updated Packages dependencies**: Updated framework packages dependencies to the latest versions.
**Updated supported target frameworks**: all packages now target `netstandard2.0` which means minimal supported .net framework version now is `net462`.

## Core

### Breaking Changes

1. **Obsolete code removal**: Removed legacy `Logger`, `TrxCrteator`, `IsPositiveMatcher`

2. **API Changes**: Some of the API methods have been changed in part of taf events. You will need to update your code to use the new API methods.

### Migration Steps

1. **Update Code**: Update your code to use the new API methods.
 - Replace `Unicorn.Taf.Core.Logging.Logger` with `Unicorn.Taf.Core.Logging.ULog` (see [documentation](../../core/logging/)).
 - `TrxCreator` is no longer used. `dotnet test` trx generation abilities could be used instead.
 - `IsPositiveMatcher` could be replaced with `IsGreaterThanMatcher`
 - Update references to TAF events for steps, suites, tests and fixtures. Now all framework events are located in single class `Unicorn.Taf.Core.TafEvents`.

2. **Update your dependencies**: Update versions of all used unicorn packages in your `*.csproj` files. (see section [V4 packages support](#v4-packages-support))

3. _Optionally could be removed inheritance of test suite classes from `TestSuite`. It's not longer necessary_.

## UI

### Breaking Changes

1. **API Changes**: Some of the API methods have been changed for Screenshotters and ImplicitWait timeouts handling. You will need to update your code to use the new API methods.

### Migration Steps

1. **Update Code**: Update your code to use the new API methods.
 - Replace `ScribeToTafEvents` screenshotters method call with `SubscribeToTafEvents`.
 - Replace `SetImplicitlyWait` method call (inherited from `UiSearchContext`) with `ImplicitWaitTimeout` property setting.


## Reporting

### Breaking Changes

1. **Packages Renaming**: Reporting packages have been renamed. You will need to update your code to use the new package names.

2. **API Changes**: Some of the API methods have been changed (reporters instances class names). You will need to update your code to use the new API methods.

### Migration Steps

1. **Update your dependencies**: Update your `*.csproj` files to use the new package names.
 - Replace `Unicorn.ReportPortalAgent` with `Unicorn.Reporting.ReportPortal` (version 4.x).
 - Replace `Unicorn.AllureAgent` with `Unicorn.Reporting.Allure` (version 3.x).

2. **Update your code**: Update your code to use the new APIs.
 - Replace `ReportPortalReporterInstance` with `ReportPortalReporter` (see [documentation](../../reporting/report-portal-integration/)).
 - Replace `AllureReporterInstance` with `AllureReporter` (see [documentation](../../reporting/allure-report-integration/))
 - For TestIT reporter replace `ReporterInstance` with `TestItReporter`

## V4 packages support
Core V4 is supported by next unicorn packages versions:
 - Unicorn.Backend 3.0.0+
 - Unicorn.UI.Core 4.0.0+
 - Unicorn.UI.Web 4.1.0+
 - Unicorn.UI.Win 4.0.0+
 - Unicorn.TestAdapter 4.0.0+
 - Unicorn.Reporting.ReportPortal 4.0.0+
