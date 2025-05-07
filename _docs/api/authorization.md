---
title: Authorization
category: API
weight: 4
order: 4
---

Authorization is one of the basic thing need to be done when working with API. It usually involves providing some kind of session data, like token, cookies, etc. There is a built-in mechanism to handle sessions in the Backend library.

## IdentityServer

`IdentityServerClient` is used to get authorization token from IdentityClient

All is you need is to initialize the client providing base URL and client credentials:

```csharp
IdentityServerClient authClient = new IdentityServerClient("https://some-url");

// "password" grant_type is used automatically
IServiceSession session = authClient.GetTokenByPassword("username", "password");
```

Other types of auth data could be used:

```csharp
// "authorization_code" grant_type is used automatically
session = authClient.GetTokenByAuthorizationCode("some-code");

// "client_credentials" grant_type is used automatically
session = authClient.GetTokenByClientCredentials("clientId", "clientSecret");

// "scope" also could be provided
session = authClient.GetTokenByClientCredentials("clientId", "clientSecret", "scope");
```

It's possible to fully customize auth call with endpoint and key-value parameters:

```csharp
Dictionary<string, string> parameters; // custom key-value pairs
session = authClient.GetToken("custom-endpoint-url", parameters);
```

## Keycloak

`KeycloakClient` is used to get authorization token from Keycloak. To use it it's necessary to initialize the client providing base URL and your custom realm name:

```csharp
KeycloakClient authClient = new KeycloakClient("https://some-url", "your-realm-name");
```

The same auth options as for `IdentityServerClient` could be used for Keycloak.


## OAuth2 data

`OAuth2Data` object could be used to store OAuth2 data retrieval fields (grant_type, scope etc.). Both auth clients could use `OAuth2Data` object to get token:

```csharp
// Init auth data
OAuth2Data authData;

// using password grant
authData = OAuth2Data.FromPassword("username", "password");

// using client_credentials grant
authData = OAuth2Data.FromClientCredentials("clientId", "clientSecret");

// or
authData = OAuth2Data.FromClientCredentials("clientId", "clientSecret");

// using authorization_code grant
authData = OAuth2Data.FromAuthorizationCode("code");


// and get token
IServiceSession session = authClient.GetToken(authData);
```