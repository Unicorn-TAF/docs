---
title: Templates
category: Tooling
weight: 7
order: 1
---

Templates are powerful feature that allow to create ready to use items like projects or classes with prefilled code saving time on some routine operations.
To use unicorn templates install a package using `dotnet new install Unicorn.Taf.Templates`

Available templates are:

|Short Name|Template Name|
|----------|-------------|
|unicorn|Unicorn Tests project|
|unicorn-suite|Unicorn Test suite|
|unicorn-api|Unicorn API module project|
|unicorn-api-client|Unicorn API client|
|unicorn-web|Unicorn Web module project|
|unicorn-web-page|Unicorn Web page|
|unicorn-web-control|Unicorn Web user control|
|unicorn-win|Unicorn Win module project|
|unicorn-win-window|Unicorn Win (desktop) window|
|unicorn-steps|Unicorn Test steps injection project|
|unicorn-steps-class|Unicorn Test steps class|


# Tests

## Project
To create new tests project use `unicorn` template. The template provides with ability to initialize the project with built-in integration with reporting tools: Allure, ReportPortal or TestIT. Corresponding reporter initialization config template is also added if reporting integration is specified. The template creates a project with predefined `[TestsAssembly]` class, empty suite class and test suite showcase example.

Available template options:

- `-f|--framework`  
  Target framework  
  **Default**: `net8.0`

- `-r|--reporting`
  External reporting system to use  
  **Default**: `none`  
  **Available values**:
    - `none`          No reporting
    - `reportportal`  Use Report Portal as external reporting system
    - `allure`        Use Allure reports as external reporting system
    - `testit`        use TestIT as external reporting system

Examples:
```bash
# initialize new project with default name `Company.Tests` in current directory
dotnet new unicorn

# initialize new project with name `TestsProject` in TestsProject directory
dotnet new unicorn -o TestsProject

# initialize new project with name `TestsProject` in TestsProject directory with pre-initialized reporting to ReporPortal and targeting net9.0
dotnet new unicorn -o TestsProject --reporting reportportal --framework net9.0
```

## Test Suite

To create new test suite class use `unicorn-suite` template.

Available template options:

- `-p:n|--namespace`  
  The namespace to place class in.  
  **Default**: `Company.Tests`

# Steps

## Steps injection project

`unicorn-steps` template allows one to create ready for use implementation of test steps injection which could be referenced by other projects.

Available template options:
- `-f|--framework`  
  Target framework  
  **Default**: `net8.0`

Examples:
```bash
# initialize new steps injection project with name `StepsInjection` in directory StepsInjection
dotnet new unicorn-steps -o StepsInjection
```

## Steps class

To create new steps class use `unicorn-steps-class` template.

Available template options:  
**Required**
- `-in|--injection-namespace`  
  The namespace of Steps injection attribute (`StepsClassAttribute`)  
  **Default**: `Company.StepsInjection`

**Optional**
- `-p:n|--namespace`  
  The namespace to place class in.  
  **Default**: `Company.Steps`

# API

## Project

`unicorn-api` template is used to create a project for some API component. The template creates a project with predefined REST client class.

Available template options:

- `-f|--framework`  
  Target framework  
  **Default**: `net8.0`

## API client

To create new API client class use `unicorn-api-client` item template

Available template options:

- `-k|--kind`  
  Type of API client (if not specified, REST API client is created)  
  **Default**: `rest`  
  **Available values**:
    - `rest`   Create REST API client
    - `soap`   Create SOAP API client

- `-p:n|--namespace`  
  The namespace to place class in.  
  **Default**: Company.ApiModule

# Web UI

## Project

`unicorn-web` template is used to create a project for some Web component. The template creates a project with predefined Website and empty Web Page classes.

Available template options:
- `-f|--framework`  
  Target framework  
  **Default**: `net8.0`

- `-wa|--with-api`  
  The flag is used to add Unicorn.Backend dependency to the project. If flag is presented the dependency is added, otherwise - not.

Examples:
```bash
# initialize new web automation project with name `WebModule` in directory WebModule
dotnet new unicorn-web -o WebModule

# initialize new web automation project with additional dependency on Unicorn.Backend
dotnet new unicorn-web --with-api
```

## Web Page

`unicorn-web-page` template is used to create a new Web Page class with a predefined control initialization example.

Available template options:
- `-p:n|--namespace`  
  The namespace to place class in.
     

## Web control

To create custom user control use `unicorn-web-control` template. Different types of user controls could be created

Available template options:

- `-k|--kind`  
  Type of user control (if not specified, empty control is created)  
  **Default**: `none`  
  **Available values**:
    - `none`      Create custom empty control
    - `dropdown`  Create dropdown base control
    - `window`    Create window base control
    - `listview`  Create list view base control
    - `checkbox`  Create checkbox base control
    - `datagrid`  Create data grid base control

- `-p:n|--namespace`  
  The namespace to place class in.  
  **Default**: `Company.WebModule`

# Windows UI

## Project

`unicorn-win` template is used to create a project for some Windows application. The template creates a project with predefined Application and empty Window classes.

Available template options:
- `-f|--framework`  
  Target framework  
  **Default**: `net8.0-windows`

Examples:
```bash
# initialize new windows application automation project with name `WinModule` in directory WinModule
dotnet new unicorn-win -o WinModule
```

## Window class

`unicorn-win-window` template is used to create a new Window control class with a predefined control initialization example.

Available template options:

- `-p:n|--namespace`  
  The namespace to place class in  
  **Default**: `Company.WinModule`

# Complex example

Example below creates a solution with 3 projects:
1. Tests project with already initialized reporting to Allure.
2. Test steps injection project.
3. Combined Web+API project with basic controls definitions and REST client. The project references steps injection and base test steps classes are created in Web and Api namespaces

```bash
dotnet new sln -o DemoSolution
cd DemoSolution

dotnet new unicorn -o Autotests --reporting allure
dotnet sln add Autotests

cd Autotests
dotnet new unicorn-suite -o Suites -n MyNewSuite -p:n Autotests.Suites --no-update-check
cd ..

dotnet new unicorn-steps -o StepsInjection --no-update-check
dotnet sln add StepsInjection

dotnet new unicorn-web -o AppComponent --with-api --no-update-check
dotnet sln add AppComponent

cd AppComponent
dotnet add reference ../StepsInjection/StepsInjection.csproj

dotnet new unicorn-web-control -o Web/Controls -n MyDropdown --kind dropdown -p:n AppComponent.Web.Controls --no-update-check
dotnet new unicorn-web-control -o Web/Controls -n MyCheckbox --kind checkbox -p:n AppComponent.Web.Controls --no-update-check
dotnet new unicorn-web-control -o Web/Controls -n MyListView --kind listview -p:n AppComponent.Web.Controls --no-update-check
dotnet new unicorn-web-control -o Web/Controls -n MyDataGrid --kind datagrid -p:n AppComponent.Web.Controls --no-update-check
dotnet new unicorn-web-control -o Web/Controls -n SomeWindow --kind window -p:n AppComponent.Web.Controls --no-update-check
dotnet new unicorn-web-page -o Web/Pages -n MyWebPage -p:n AppComponent.Web.Pages --no-update-check

dotnet new unicorn-api-client -o Api/Clients -n MyRestClient --kind rest -p:n AppComponent.Api.Clients --no-update-check

dotnet new unicorn-steps-class -o Web/Steps -n WebSteps -in StepsInjection -p:n AppComponent.Web.Steps --no-update-check
dotnet new unicorn-steps-class -o Api/Steps -n ApiSteps -in StepsInjection -p:n AppComponent.Api.Steps --no-update-check
```