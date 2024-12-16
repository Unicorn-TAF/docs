---
title: Test adapter
category: Testing
order: 5
---

The adapter allows to run unicorn tests directly from Visual Studio or through dotnet test. To install test adapter just add `Unicorn.TestAdapter` dependency to project containing tests.

## Use of .runsettings
### Use of unicorn configuration file
To use custom unicorn settings file when running tests via tests explorer just add next section to _.runsettings_ file

```xml
<UnicornAdapter>
  <ConfigFile>unicornConfig.json</ConfigFile>
</UnicornAdapter>
```

### Results directory
By default test adapter runs tests from build output directory and stores results there.
It's possible to change defaults using _.runsettings_. It could be done by adding:

```xml
<UnicornAdapter>
  <ResultsDirectory>.\TestResults</ResultsDirectory>
</UnicornAdapter>
```

In this case **TestResults** directory will be created in solution root. Each tests launch will have its own directory named by template `{MachineName}_{MM-dd-yyyy_hh-mm}`

## dotnet test
Unicorn test adapter also allows to run tests using `dotnet test` CLI command. 

### Filtering
Next properties are available for filtering:  
 - **DisplayName**: Unicorn test title
 - **FullyQualifiedName**: Full test method name including namespace and type
 - **Tag**: Parent suite tags list (match of any tag from list)
 - **Category**: Test categories list (match of any category from list)

### Examples
```bash
# print to console all tests whose parent suite has any tag containing "web"
dotnet test -t --filter Tag~web

# print to console all tests which have category "Smoke"
dotnet test -t --filter Category=Smoke

# run all tests which FullyQualifiedName does not contain "WebTests"
dotnet test --filter FullyQualifiedName!~WebTests

# run all tests and save results in .trx file. To run tests use specified runsettings file
dotnet test --logger "trx;logfilename=testResults.trx" --settings <SETTINGS_FILE>

# run all ApiTests tests on Release configuration skipping build and with detail logging in console
dotnet test --no-build -c Release --logger "console;verbosity=detailed" --filter Category=ApiTests
```