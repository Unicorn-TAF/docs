---
title: Dataset Generator
category: "Helpers & utilities"
weight: 7
order: 3
---

## Overview
The `DataSetGenerator` is a static utility class that provides syntactically convenient methods to generate test data sets lists based on common test data scenarios. It simplifies the creation of `DataSet` objects for parameterized testing.

Namespace: `Unicorn.Taf.Core.Utility`

## Usage Examples

### Single Value Data Sets
```csharp
// Generate a data set from a list of values
private static List<DataSet> TestData() => 
    DataSetGenerator.FromItems(10, 20, 30, 40, 50);

[Test("Single item dataset")]
[TestData(nameof(TestData))]
public void SingleItemDatasetTest(int value)
{
    // Test logic
}
``` 

### Pairwise Combinations
```csharp
// Test matrix for login scenarios (pairs Username/Password):
// 
// admin/correct
// admin/wrong
// admin/empty
// user/correct
// user/wrong
// user/empty
// guest/correct
// guest/wrong
// guest/empty
private static List<DataSet> LoginTestData() => 
    DataSetGenerator.CombinationOf(
        new[] { "admin", "user", "guest" }, // logins
        new[] { "correct", "wrong", "empty" } // passwords
    );

[Test("Pairwise combination item dataset")]
[TestData(nameof(LoginTestData))]
public void PairwiseCombinationsTest(string username, string password)
{
    // Test login with different combinations
}
```

### Three-Way Combinations
```csharp
// Test matrix for Three-Way Combinations (pairs: country / tier / device):
// 
// USA / Gold / Desktop
// USA / Silver / Desktop
// ...
// Germany / Basic / Desktop
// Germany / Basic / Mobile
private static List<DataSet> PricingTestData() => 
    DataSetGenerator.CombinationOf(
        new[] { "USA", "UK", "Germany" },      // Countries
        new[] { "Gold", "Silver", "Basic" },   // Membership tiers
        new[] { "Desktop", "Mobile" }           // Device types
    );

[Test("Three-Way Combinations")]
[TestData(nameof(PricingTestData))]
public void TestPricing(string country, string tier, string device)
{
    // Test pricing logic across multiple dimensions
}
```
