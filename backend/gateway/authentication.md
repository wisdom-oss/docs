# Authentication

The API gateway is shipped with many possibilities for managing authentication
directly in the gateway. 
However, the integration with an external OpenID Connect provider is a premium
feature thats only available when subscribing to the enterprise plans.
Therefore, a [custom plugin](https://github.com/wisdom-oss/gateway-oauth-plugin)
has been written which uses the `userinfo` endpoint of the configured IDP to
get the users information and check if the user really is authenticated.

The authentication is automatically configured by the service watcher to use
the `INTROSPECTION_URL` present in its environment.
This value is controlled in the `wisdom.conf` file in the directory of your
deployment files.

## What does the plugin do?

The plugin attaches itself to the whole gateway.
This results in the need for authentication for every route that is managed by
the gateway. 
This has been done to ensure a safe operation and minimize unauthenticated
access to the microservices behind the gateway.

On a incoming request, the plugin takes that request and checks, if the
`Authorization` header contains an Bearer Token[^1].
If the request has no `Authorization` header set, it will reject the request
and indicate, that the user need to authenticate before accessing the gateway.
If a value has been found and extracted, it will be used to request the
`userinfo` endpoint of the configured identity provider.

The result of that request is then translated into headers, that will be added
to the request in the downstream.

[^1]: https://datatracker.ietf.org/doc/html/rfc6750
