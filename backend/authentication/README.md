---
title: Authentication Management
---

<div align="center">
<a href="https://goauthentik.io">
<img height="150px" src="https://raw.githubusercontent.com/goauthentik/authentik/main/web/icons/icon_left_brand.svg"/>
</a>
<hr/>
</div>

The WISdoM platform utilizes authentik as a single sign-on solution for the
frontend and the backend.
It features a user management to create users directly in authentik, provides
a OAuth 2.0/OpenID Connect interface for the frontend to use it as main
authentication platform and allows to set up external user sources like
LDAP directories, AzureAD or other OAuth 2.0/OpenID Connect enabled sources.<br/>
[_Read more_](https://goauthentik.io)

In general, the project uses OAuth 2.0[^1] and OpenID Connect[^2] for supplying
authentication and authorization information.

[^1]: The OAuth 2.0 authorization framework enables a third-party
   application to obtain limited access to an HTTP service, either on
   behalf of a resource owner by orchestrating an approval interaction
   between the resource owner and the HTTP service, or by allowing the
   third-party application to obtain access on its own behalf.<br>
   Read more here: [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)

[^2]: OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 
    protocol. It enables Clients to verify the identity of the End-User based on 
    the authentication performed by an Authorization Server, as well as to 
    obtain basic profile information about the End-User in an interoperable and 
    REST-like manner.<br>
    Read more here: [OpenID Connect Core Specification](https://openid.net/specs/openid-connect-core-1_0.html)


## Frontend Authentication
To access the frontend, a user needs to be registered on the identity provider
it has been configured for during the [deployment](../deployment.md).
It automatically redirects any unauthorized user to the login page of the
identity provider and requires them to complete the login process.
After the login has been successful, the frontend get an access token to allow
the frontend user to make requests to the backend.

## Backend Authentication
To access the backend, a user needs to use a bearer token as defined by 
[RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750) in the `Authorization`
header. The access token then is validated in the API gateway using a custom
plugin which tries to request the `userinfo` endpoint of the identity provider
to check if the user is authenticated and to receive the groups the user is in
to allow the management of authorization in the microservices. Furthermore,
the user is also identified and the user name is attached to a request to
let the microservice know who accesses it.

Read more about the authorization in microservices [here](./microservice.md)

Read more about the authentication in the api gateway [here](../gateway/authentication.md)
