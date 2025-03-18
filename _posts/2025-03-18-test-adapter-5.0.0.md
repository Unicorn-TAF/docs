---
title: TestAdapter 5.0.0
type: major
---

The release addressed to removing of dependency on `BinaryFormatter` and corresponding vulnerability [CWE-502](https://cwe.mitre.org/data/definitions/502.html), now `JsonSerializer` is used instead to pass data between load contexts in netcore implementation.

**[BREAKING]** Support only of of Taf.Core v4.2.0+

**Features:**

* Added test attachments to test results
* `TestOutcome.Output` is reported as **Standard Output** in test results

**Fixes:**

* `BinaryFormatter` replaced with `JsonSerializer` to pass data between load contexts in netcore implementation 