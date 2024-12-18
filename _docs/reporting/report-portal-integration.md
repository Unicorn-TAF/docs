---
title: ReportPortal integration
category: Reporting
weight: 6
order: 2
---

Place `ReportPortal.config.json` configuration file to directory with test assemblies. Sample content is presented below:

```json
{
    "enabled": true,
    "server": {
        "url": "https://report-portal-uri/api/v1/",
        "project": "Some project",
        "authentication": {
        "uuid": "your_uuid"
        },
    },
    "launch": {
        "name": "Unicorn tests run",
        "description": "Unit tests of Unicorn Framework",
        "debugMode": false,
        "tags": [ "Windows 10", "UnicornFramework" ]
    }
} 
```

Initialize the reporter in TestAssembly initialization

```csharp
using Unicorn.Core.Testing.Tests.Attributes;
using Unicorn.Reporting.ReportPortal;
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
            // Start new launch in Report Portal.
            reporter = new ReportPortalReporter(); 
            
            // in case you want to report into already started existing launch use
            // reporter = new ReportPortalReporter(existing_launch_id);
        }

        [RunFinalize]
        public static void FinalizeRun()
        {
            // Finish launch in Report portal if it was not externally started.
            reporter.Dispose(); 
            reporter = null;
        }
    }
}
```