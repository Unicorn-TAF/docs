---
title: TestIT integration
category: Reporting
weight: 6
order: 3
---

Place `Tms.config.json` configuration file to directory with test assemblies. Sample content is presented below:

```json
{
    "url": "url_to_testit_instance",
    "privateToken": "token_value",
    "projectId": "project_id_value",
    "configurationId": "configuration_id_value",
    "testRunId": "id_of_started_test_run (if id not specified, new run starts automatically)",
    "testRunName": "custom_run_name_in_case_testRunId_is_not_specified",
    "automaticCreationTestCases": false,
    "automaticUpdationLinksToTestCases": false,
    "certValidation": true,
    "isDebug": false,
}
```

Initialize the reporter in TestAssembly initialization

```csharp
using Unicorn.Core.Testing.Tests.Attributes;
using Unicorn.Reporting.TestIt;
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
            // Initialize built-in TestIT reporter with automatic subscription to all testing events.
            // Tms.config.json should exist in binaries directory.
            reporter = new TestItReporter(); 
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