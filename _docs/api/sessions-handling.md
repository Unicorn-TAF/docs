---
title: Sessions handling
category: API
weight: 4
order: 2
---

Usually to get response from API endpoint it's necessary to provide some kind of session data, like token, cookies, etc. There is a built-in mechanism to handle sessions in the RestClient. 

## Custom session and session management

RestClient allows to use custom session data. To do this, you need to implement `IServiceSession` interface and provide it to the RestClient. If `RestClient.Session` property is initialized, the session is used to populate each request to be sent with necessary session data (this logic has to be implemented in `IServiceSession.UpdateRequestWithSessionData` for the custom session). Here is an example of how to do it:

```csharp
// Custom session should implement IServiceSession
public class CustomSession : IServiceSession
{
    private readonly string _token;
    
    // let's consider the token was already retrieved by some way
    public CustomSession(string token)
    {
        _token = token;
    }

    // This method from IServiceSession should be implemented 
    // here based on specifics of target service authorization 
    // initial HttpRequestMessage should be populated with session data.
    public void UpdateRequestWithSessionData(HttpRequestMessage request)
    {
        request.Headers.Add("Authorization", $"Api-Token {_token}");
    }
}

public class CustomApiClient : RestClient
{
    // Just need to init the client with the session and to set Session property
    public CustomApiClient(CustomSession customSession) : base("https://some-url")
    {
        Session = customSession;
    }
}
```