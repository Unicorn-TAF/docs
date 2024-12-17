---
title: Assertions on API responses
category: API
order: 3
---

[Unicorn.Backend](https://www.nuget.org/packages/Unicorn.Backend) has collection of rest api matchers compatible with [core assertions mechanism](../../testing/assertions). The matchers can be used to validate the response of the API: status code, response body, response headers, and more. Here is an example of how to use these assertions:

```csharp
RestResponse userResponse; // get response from the API

Assert.That(userResponse, 
    Service.Rest.Response.HasStatusCode(HttpStatusCode.OK));

Assert.That(userResponse, 
    Service.Rest.Response.ContentContains("some_expected_part"));

Assert.That(userResponse, 
    Service.Rest.Response.HasTokenWithValue("$.data.id", 2));

Assert.That(userResponse, 
    Service.Rest.Response.EachTokenHasChild("$.data", "id"));

Assert.That(userResponse, 
    Service.Rest.Response.HasTokensCount("$.data.id", 1));

Assert.That(userResponse, 
    Is.Not(Service.Rest.Response.HasTokenMatchingJsonPath("$.data.id.asd.asd")));
```