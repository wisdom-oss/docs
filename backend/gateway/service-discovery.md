# Service Discovery
To allow the dynamic adding and removing of services based on their current
execution status (running, unhealthy, healthy, stopped) a service has been
developed, which uses the docker socket to scan the host it is running on.

The scanner relies on the concept of labels for docker containers. 
They are used to attach additional information on a container.

It checks the docker host for containers that have the following labels set:
* `wisdom-oss.isService`
* `wisdom-oss.service.name`
* `wisdom-oss.service.path`
* `wisdom-oss.service.upstream-name`
* `wisdom-oss.service.healthcheck`

If the scanner finds a container it is inspected further and afterwards either
registered on the gateway or removed from the gateway, depending on the current
state.

This check is executed every 5 seconds to be able to pick up on changes in a
services availability fast.