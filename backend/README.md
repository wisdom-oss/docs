# Backend Overview

The following graph shows how a request is processed by the backend
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


    
    