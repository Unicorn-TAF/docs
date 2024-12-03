---
title: Taf.Core 4.0.0
type: major
---

Major release of Taf.Core library

**Features:**

* **[BREAKING]** The library retargeted to netstandard2.0
* **[BREAKING]** Removed obsolete code (`Logger`, `IsPositiveMatcher`, `TrxCreator`)
* **[BREAKING]** Get rid of mandatory inheritance of test suites classes from `TestSuite`, suite name now is not mandatory parameter of `[Suite]`
* **[BREAKING]** All framework events combined at one place `TafEvents`
* Added test step fail event `OnStepFail`
* Improved error message in case of test skip to make it easier to determine skip reason
* Added `OfType` matcher
* Added Readme

**Fixes:**

* Fixed issue when custom format is ignored in Step description
