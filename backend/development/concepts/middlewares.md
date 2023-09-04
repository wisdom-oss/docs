# Middlewares
Middlewares are a concept of the router used in the microservices called
[`go-chi/chi`](https://github.com/go-chi/chi).
The middlewares are able to modify and handle requests before they reach the
handler function written to actually handle the request and are utilized in the
WISdoM project to enable a more unified code base between services.

## Default Middlewares
By default the following middlewares included in the `go-chi/chi` package are
used:
* [`go-chi/chi/middleware.RealIP`](https://pkg.go.dev/github.com/go-chi/chi/middleware#RealIP)
    This middleware sets the remote address of the http request to the value of
    the `X-Forwarded-For` or the `X-Real-IP` header and therefore allows the
    loggers to see the real ip of the request.

* [`go-chi/chi/middleware.RequestID`](https://pkg.go.dev/github.com/go-chi/chi/middleware#RequestID)
    This middleware adds an request ID to every request that allows the
    identification of the request in logs and allows to identify the container
    and process that handled the request.

* [`go-chi/chi/middleware.Compressor`](https://pkg.go.dev/github.com/go-chi/chi/middleware#Compressor)
    This object is responsible for managing the compression of responses to make
    the amount of data transferred smaller to allow for faster response times.
    The compressor object is configured to use `gzip` and `brotli` as 
    compression algorithms.

    Calling the [`Handler()`](https://pkg.go.dev/github.com/go-chi/chi/middleware#Compressor.Handler)
    function returns the handler used as the middleware.

> The inner workings of these middlewares will not be discussed here. However, the
> code for each middleware is available in [this repository](https://github.com/go-chi/chi/tree/master/middleware)

## Custom Middlewares
### Preimplemented Middlewares
Since every microservice needs some core functionalities implemented in the same
way to allow a consistent output in some matters, there are three preimplemented
middlewares available:

* [`wisdomMiddleware.Authorization`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#Authorization) 
    This middleware handles the reading and parsing of the [authorization headers
    set by the API gateway](../../gateway/authorization.md). This should always
    be the first middleware loaded after the middlewares listed under the
    default middlewares.<br/>
    _[Read more](./authorization.md)_

* [`wisdomMiddleare.NativeErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#NativeErrorHandler) 
    This middleware adds two channels to the requests context that enable the
    handling of a native golang error by putting them into one of the channels
    while listening on the completion on another one.<br/>
    _[Read more](./error-handling.md)_    

* [`wisdomMiddleare.WISdoMErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#WISdoMErrorHandler) 
    This middleware adds two channels to the requests context that enable the
    handling of custom predefined errors.<br/>
    _[Read more](./error-handling.md)_   

### Writing yor own middleware
🚧🚧🚧🚧 This is a work in progress!
