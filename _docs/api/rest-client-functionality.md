---
title: Rest API base functionality
category: API
weight: 4
order: 1
---
From the box [Unicorn.Backend](https://www.nuget.org/packages/Unicorn.Backend) provides with base rest service implementation which could help top send requests to the API and get responses. 

## Usage

### Sending requests
To use the base functionality you need to create an instance of `RestClient` class and call `SendRequest` method. This method takes `HttpMethod` and `endpoint` as parameters and returns `RestResponse` object. 
Here is an example of how to use it:

```csharp
using System.Net.Http;
using Unicorn.Backend.Services.RestService;

// Create new base client instance
RestClient client = new RestClient("https://reqres.in");

// Send get request to the API endpoint and get response object
RestResponse response = client.SendRequest(HttpMethod.Get, "/api/users/1");
```

`RestResponse` contains set of basic properties related to the response such as `StatusCode`, `Content` and `Headers`. You can use these properties to process the response data.
 - **ExecutionTime**: request execution time
 - **Status**: response status code
 - **StatusDescription**: response status description
 - **Content**: response content as string
 - **Headers**: response headers as `HttpResponseHeaders`
 - **AsJObject**: response content as `JObject` (Newtonsoft.Json library)


### Download files
To download files you can use `DownloadFile` method. This method takes `endpoint` and `destinationFolder` as parameters and returns `string` with original filename. Filename is taken from `Headers.ContentDisposition.FileNameStar`, in case it's not specified endpoint tail part is used as filename.

Here is an example of how to use it:

```csharp
string fileName = new RestClient("https://some-url")
    .DownloadFile("/api/download-endpoint", "target_dir_path");
```


## Custom API client
It's possible to create custom API client based on `RestClient` class. You can override some properties and methods to customize behavior of the client. Here is an example of how to create custom API client:

```csharp
public class DummyApiClient : RestClient
{
    // Initializing an instance of DummyApiClient calling base constructor with api base url.
    public DummyApiClient() : base("https://reqres.in")
    {
    }

    // disable auto-redirects for all requests made by this client.
    protected override bool AllowAutoRedirect => false;

    // set encoding to ASCII for all requests made by this client.
    protected override Encoding Encoding => Encoding.ASCII;

    // Call get on "/api/users/{id}" endpoint and receive a response
    public RestResponse GetUser(int id) =>
        SendRequest(HttpMethod.Get, $"/api/users/{id}");
}
```