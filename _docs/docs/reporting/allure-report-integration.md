---
title: Allure report integration
category: Reporting
order: 1
---

Place allureConfig.json configuration file to directory with test assemblies. Sample content is presented below:

```json
{
    "allure": {
        "directory": "path_to_directory_with_report",
        "links": [
            "https://github.com/Unicorn-TAF/examples/issues/{issue}",
            "https://some-tms-url/{tms}",
        ]
    }
}
```

Initialize the reporter in TestAssembly initialization

```csharp
using Unicorn.Core.Testing.Tests.Attributes;
using Unicorn.Reporting.Allure;
using Unicorn.Taf.Api;

namespace Tests
{
    [TestsAssembly]
    public static class TestsAssembly
    {
        private static ITestReporter reporter;

        [RunInitialize]
        public static void InitRun()
        {
            // Initialize built-in Allure reporter with automatic subscription to all testing events.
            // allureConfig.json should exist in binaries directory.
            reporter = new AllureReporter();
        }

        [RunFinalize]
        public static void FinalizeRun()
        {
            reporter.Dispose(); 
            reporter = null;
        }
    }
}
```