# Authentication/Authorization for microservices

The microservices running in the backend of the WISdoM platform generally can
enforce two types of authorization and authentication.
The first one being that the user only needs to  be authenticated and the 
second one being that the user needs to be explicitly authorized to use the 
microservice.

If the microservice only requires the user to be authenticated, it will allow
every request that has passed the API gateway and the [there configured 
authentication procedures](../gateway/authentication.md).
The determination, if the request passed the API gateway is made by checking
the value of the `X-WISdoM-Gateway-Passed` header.
To secure the microservices against an attacker setting this header from the
public side of the platform, the [reverse proxy](../reverse-proxy/README.md)
filters this header and removes it from every request.

If the microservice requires the user to be authorized, it will check the
`X-WISdoM-Gateway-Passed` header and it will check the groups set
in the  `X-WISdoM-Groups` header. The values of the two
headers are processed by the [`wisdomMiddleware.Authorization`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#Authorization) 
middleware and the request is either passed to the next handler or rejected
by the middleware, resulting in an error message being sent back.

The `X-WISdoM-User` header is always required even if no user was identified. If
the header is missing, the authorization middleware will reject the request with
a status `400 Bad Request`, since the user header is not existing.
> This always applies, regardless of the configuration 

The `X-WISdoM-Admin` header defines, if a user is currently marked as an
administrator in the identity provider. If the value of the header, matches the
username set in `X-WISdoM-User` the microservice will allow the access to the
microservice, without regard the the groups of the user requesting data from
the microservice.
> This only applies if the microservice enforces authorization

## Configure the microservice
The authentication/authorization of microservices is directly configured by the
`resources/authConfig.json` file in the microservice. Per default the template
service is configured only require authentication, not authorization.

To require users to be authorized for the service, you need to set the value
of `requireAuthorization` to `true`. Furthermore, you need to specify the groups
that the user needs to be in, to gain access to the microservice. If no groups
are specified, the microservice is locked for non-administrative users, since
the list of allowed groups is empty!

If no configuration file can be found, the service automatically assumes, that
only authentication is required to access the microservice. This behavior is
implemented in the initialization process of the microservice template.

<details>
<summary>Default Configuration</summary>

```json
{
  "requireAuthorization": false,
  "allowedGroups": []
}
```
</details>