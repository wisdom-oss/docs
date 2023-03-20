# Backend Overview
The Backend consists of three major parts:
 * [Kong API Gateway](kong.md) 
   ([ext. Website](https://docs.konghq.com/gateway/latest/))
 * [Authentik](authentik.md) 
   ([ext. Website](https://goauthentik.io/docs/))
 * [Microservices](microservices.md)

All three parts and their dependencies are running in Docker Containers managed
by [Docker Compose](https://docs.docker.com/compose/) using a 
[docker-compose.yml](https://github.com/wisdom-oss/deployment/blob/main/docker-compose.yml)
file.
> 🚨 The linked `docker-compose.yml` file is currently under rework. The content
> of this file may change or be temporarily unavailable.

## Kong API Gateway
![Docker Image Version (tag latest semver)](https://img.shields.io/docker/v/_/kong/alpine?style=for-the-badge)
<a href="https://docs.konghq.com/gateway/latest/">![Documentation Link](https://img.shields.io/badge/External%20Docs-docs.konghq.com%2Fgateway%2Flatest%2F-informational?style=for-the-badge)</a>

The Kong API Gateway is responsible for routing incoming requests to the
microservices after validating the authorization contained in the request at
the authentik service.

<details>
<summary> Example Request Flow</summary>

```mermaid
sequenceDiagram
    actor u as User
    participant caddy as Http Entrypoint
    participant api as Kong API Gateway
    participant auth as Authentik
    participant s as Microservice
    
    u->>+caddy: New API Request
    caddy->>+api: Route request
    critical Get Userinfo
        api->>+auth: Get /userinfo
        auth-->>-api: userinfo    
    end
    api-->api: Check authorization
    alt valid autorization
        api->>+s: Route request
        s-->s: Handle Request
        s-->>-api: response
    else invalid authorization
        api-->>-caddy: send error
    end
    caddy-->>-u: response
```
</details>