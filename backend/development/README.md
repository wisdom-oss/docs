# Overview

This section contains information about the prerequisites and important concepts
needed to develop a new microservice for the WISdoM platform.
Furthermore, this page contains some important information and recommended ways
to access some services and how to develop your microservice in a secure
manner.

## Connecting to services running on a remote host
> If you need to connect to another microservice, please use the API gateway and
> generate an access token via the web interface. The port forwarding described 
> here is only applicable to services like databases and message brokers.
>
> A guide on how to generate an access token via the browser is detailed 
> [here](./generate-api-token).

To connect your microservice to a service running on a remote host it is
recommended to use SSH to connect to the remote host and use port forwarding
to allow your local machine to accept traffic on a port which then is forwarded
to the selected port on the remote machine.

> ✨ To make SSH connections more secure and to "skip" the step of entering a
> password every time, you should generate an SSH key and use this to connect to
> your remote host.

> 🦺 Verify the host you are connecting to. If you create a port forwarding to
> the wrong host, sensitive data could be leaked and your service may be in
> danger.

If you opened a connection to your server and used port forwarding all traffic
reaching the local port is forwarded to the remote port.

### Database (PostgreSQL)
Since the database container has no public port sharing configured by default,
it is impossible to access the database while developing a new microservice 
which uses the database.

To access the database from a remote host, you need to use SSH port forwarding
to route a port from your machine to the remote host on which the platform is
installed.

In this instance the following command makes the database available on your
machine under the port `5432`:
```sh
ssh -L 5432:127.0.0.1:5432  $USER@$REMOTE_HOST
```

### Cache (Redis)
If your microservice utilizes the Redis database as a cache and your development
is at a stage in which the redis database will be actively used, you can use
the following command to route the redis database from the remote machine to
you local machine under the standard port of `6379`:
```sh
ssh -L 6379:127.0.0.1:6379  $USER@$REMOTE_HOST
```

### Miscellaneous Ports
If your microservice utilized any other software that is hosted on a
different machine without any ports published, you may use the following
syntax to forward the port from the remote host to your local machine:
```sh
ssh -L $LOCAL_PORT:127.0.0.1:$REMOTE_PORT $USER@$REMOTE_HOST
```
