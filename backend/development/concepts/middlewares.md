# Middlewares
Middlewares are a concept of the router used in the microservices called
[`go-chi/chi`](https://github.com/go-chi/chi).
The middlewares are able to modify and handle requests before they reach the
handler function written to actually handle the request and are utilized in the
WISdoM project to enable a more unified code base between services.

## Default Middlewares
By default, the following middlewares included in the `go-chi/chi` package are
used:
* [`go-chi/chi/middleware.RealIP`](https://pkg.go.dev/github.com/go-chi/chi/middleware#RealIP)
    This middleware sets the remote address of the http request to the value of
    the `X-Forwarded-For` or the `X-Real-IP` header and therefore allows the
    loggers to see the real ip of the request.

* [`go-chi/chi/middleware.RequestID`](https://pkg.go.dev/github.com/go-chi/chi/middleware#RequestID)
    This middleware adds a request ID to every request that allows the
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

## Custom Middleware
### Pre-Implemented Middleware
Since every microservice needs some core functionalities implemented in the same
way to allow a consistent output in some matters, there are three pre-implemented
middlewares available:

* [`wisdomMiddleware.Authorization`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#Authorization) 
    This middleware handles the reading and parsing of the [authorization headers
    set by the API gateway](../../gateway/authorization.md). This should always
    be the first middleware loaded after the middlewares listed under the
    default middlewares.<br/>
    _[Read more](./authorization.md)_

* [`wisdomMiddleware.NativeErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#NativeErrorHandler) 
    This middleware adds two channels to the requests context that enable the
    handling of a native golang error by putting them into one of the channels
    while listening on the completion on another one.<br/>
    _[Read more](./error-handling.md)_    

* [`wisdomMiddleware.WISdoMErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#WISdoMErrorHandler) 
    This middleware adds two channels to the requests context that enable the
    handling of custom predefined errors.<br/>
    _[Read more](./error-handling.md)_   

### Writing Your Own Middleware
Since a microservice may make use of a customized middleware, you may also
write your own middlewares.
Since middlewares are acting on all requests, make sure that the middleware
you are about to write is applicable for all requests. 
An example of a middleware that fits these criteria is a middleware generating
a cache key that is attached to the requests [context](./context.md) which
then is used to check a cache (e.g. Redis) if a value with this key exists and
returns the cached response.

When implementing your middleware it needs to return a function implementing the
[`http.Handler`](https://pkg.go.dev/net/http#Handler) interface. 
The easiest way to achieve this is to use the following way of writing your 
middleware.
```go
// MyMiddleware ...
func MyMiddleware(next http.Handler) http.Handler {
    // the parameter next is the next handler that shall be executed after this
    // middleware. This may be another middleware or a request handler. There
    // is no way to detect this!
    
    // now create a new function
    fn := func(w http.ResponseWriter, r *http.Request) {
        // TODO: Implement your middleware here

        // call the next handler if no response has been returned by the
        // middleware
        next.ServeHTTP(w,r)
    }

    // now make the function a http.Handler and return this handler
    return http.HandlerFunc(fn)
}
```
With this way of coding you are able to create your own middlewares pretty fast.
For advanced pattern like supplying arguments to your middleware you could look
up how the middlewares in the `go-chi/chi` package are implemented.
