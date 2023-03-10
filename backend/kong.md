# API Gateway and Service Registration
The project uses the Kong API Gateway as API Gateway.
The gateway is extended by a plugin which allows the gateway to request the
userinfo endpoint of the authentik server.

## Service Registration
The project uses a worker container which scans the docker host for services
which are marked with the label `wisdom-oss.isService` set to `true`.

<details>
    <summary>Sequence Diagram</summary>


```mermaid
sequenceDiagram
    participant dh as Docker Host
    participant sw as Service Watcher
    participant kg as Kong Gateway

    sw->>dh: Get all containers with "wisdom-oss.isService" as label
    dh-->>sw: List of Containers
    sw-->sw: Filter containers and request info/read from labels
    sw->>kg: Register Service

```
</details>