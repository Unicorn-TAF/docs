---
title: Utilities
category: API
weight: 4
order: 5
---

## UrlBuilder

`UrlBuilder` is a utility class that helps to build URLs dynamically. It allows you to append query parameters, fragments, and paths to the base URL. `Build` method returns the final URL as a string.

### Usages

```csharp
string path;

// https://base-url:8080/endpoint
path = new UrlBuilder("https://base-url:8080")
    .Append("endpoint")
    .Build();

// https://base-url:8080/endpoint?key=value
path = new UrlBuilder("https://base-url:8080")
    .Append("endpoint")
    .Query("key", "value")
    .Build();

// https://base-url:8080/endpoint?key=value&ids=1&ids=2&ids=3
path = new UrlBuilder("https://base-url:8080")
    .Append("endpoint")
    .Query("key", "value")
    .Query("ids", new string[] {"1", "2", "3"})
    .Build();

// https://base-url:8080/endpoint#inner-atchor
path = new UrlBuilder("https://base-url:8080")
    .Append("endpoint")
    .Fragment("inner-atchor")
    .Build();


// all items added are URL encoded
path = new UrlBuilder("https://base-url")
    .Append("endpoint%")
    .Query("param", "value with spaces")
    .Build();

// will return https://base-url:8080/endpoint%25?param=value+with+spaces
```
