---
title: API Gateway
---

<div align="center">
<a href="https://konghq.com">
<img height="150px" src="https://raw.githubusercontent.com/Kong/docs.konghq.com/main/app/assets/images/Kogo-gradient.svg"/>
</a>
<hr>
</div>

The WISdoM platform utilizes the [Kong API Gateway](https://konghq.com/install#kong-community), 
in its community edition to allow the dynamic routing of requests to the single
microservice instances.
It features multiple protocols, including REST and gRPC as well as a automatic
circuit breaker for unhealthy upstream targets.

The WISdoM platform extends the API gateway with a custom plugin, which allows
the authentication of users directly in the gateway (which normally would be an
enterprise level feature) and adds a [service discovery](./service-discovery.md)
using a docker container.

## Concepts
The API gateway features multiple concepts to allow routing to the microservices
behind the gateway.
These concepts will be explained here.
The url paths are referenced against the administrative port of the API gateway
which usually is not publicly available. To connect to the port, look up the
[Development Overview](../development/README.md) as the ways on how to connect
to a service on the remote host are explained there

### Upstream
A upstream is used to collect targets (in this case microservices) to which a
request can be routed to, if a request reaches the upstream.
A upstream may contain internal as well as external targets.
A target is selected by using the Round-Robin-Algorithm

### Target
A target is a single host that a request is routed to. A target is always a
entry of a upstream. 
There are no targets outside of a upstream.

### Service
A service is _not_ a microservice for the gateway.
It just is a entity for the gateway, on which things like plugins and other
rules may be applied to that are valid, when accessing the service.
A service always needs a host to be set. 
If the host is the name or ID of a upstream entity, all requests targeted at the
service are sent to the upstream for further handling.

### Route
A route entity sets the path for a service under which it is reachable when 
accessing the gateway. It may contain multiple paths and may be tagged to allow
an easier management in large deployments or when the number of possible routes
is increasing
