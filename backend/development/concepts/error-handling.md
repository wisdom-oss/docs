# Error Handling
To allow a unified error handling throughout all microservices, the
[`wisdomMiddleware.NativeErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#NativeErrorHandler) 
and the
[`wisdomMiddleware.WISdoMErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#WISdoMErrorHandler) 
allow a consistent way of handling errors that occur during the handling of a
request.
These middlewares append channels to the [context](./context.md) of a request to 
allow a handler to write an error to the middlewares and let them directly
act upon the supplied error.

## Basics
When writing your handlers in Go, you will encounter the built-in `error`
type of Go. Co code usually uses these `error` values to indicate that an
abnormal state has been encountered. 
Since function returns in Go always need to be handled, errors need to be 
handled as well.

If a functions return values may contain an error, it always will be an error
_only_ implementing the `error` interface, even though the function internally
uses a different interface or type. 
Because of this, some errors might be asserted as a different type using the 
[`errors.As`](https://pkg.go.dev/errors#As) function as packages may use a
custom error implementation to transfer status codes or other useful 
information.
If a function returns its values and no error occurred, the value of the
returned error is `nil`. With this information, it now is possible to check if
an error ever occurred using the following code snippet:
```go
err := thisFunctionReturnsAnError()
// now test if the function really returned an error
if err != nil {
    // handle the error...
}
// continue with the code
```


## Handling a Native Error
After the basics gave an overview on how errors work in Go, this section will
now discuss, how to even handle an error if a function returned one.

Before the error can be handled, the channels inserted by the middleware need
to be retrieved in the handler you are about to encounter possible errors in
```go
func MyHandler(w http.ResponseWriter, r *http.Request) {
    // get the error channel for native errors
    nativeErrorHandler := r.Context().Value("nativeErrorChannel").(chan error)
    // get the channel that will indicate that an error has been handled
    nativeErrorHandled := r.Context().Value("nativeErrorHandled").(chan bool)

    // your code will go after these lines
}
```

After getting the two channels from the context of the request, we now jump
back into the code snippet from the previous section.
The snippet will now be extended by the error handling using the two channels.

```go
err := thisFunctionReturnsAnError()
// now test if the function really returned an error
if err != nil {
   // to provide a better output for the user on the other end, wrap the error
   // that has been returned and write some additional information
   // take the result of the wrapping and directly write it into the channel
   nativeErrorHandler <- fmt.Errorf("unable to execute function: %w", err)

   // now listen on the other channel to wait for the error to be handled
   // this blocks the execution of further code until the error has been handled
   <-nativeErrorHandled

   // since the error has been handled, exit the handler
   return
}
// continue with the code
```

Now the two snippets in this section can be merged and a basic handler which can
handle errors has been created.
```go
func MyHandler(w http.ResponseWriter, r *http.Request) {
    // get the error channel for native errors
    nativeErrorHandler := r.Context().Value("nativeErrorChannel").(chan error)
    // get the channel that will indicate that an error has been handled
    nativeErrorHandled := r.Context().Value("nativeErrorHandled").(chan bool)

    // your code will go after these lines
    err := thisFunctionReturnsAnError()
    // now test if the function really returned an error
    if err != nil {
        // to provide a better output for the user on the other end, wrap the error
        // that has been returned and write some additional information
        // take the result of the wrapping and directly write it into the channel
        nativeErrorHandler <- fmt.Errorf("unable to execute function: %w", err)

        // now listen on the other channel to wait for the error to be handled
        // this blocks the execution of further code until the error has been handled
        <-nativeErrorHandled

        // since the error has been handled, exit the handler
        return
    }
}
```

> ⚠️ This example will not work if the code is just copied from these 
> instructions

## Handling Custom Errors
> ⚠️ Do not create custom errors for errors that can be handled using the native
> error handler

Since not only internal errors, while using other functions, can occur in your
microservice, there is a second type of errors that have a middleware to handle
them.
These are errors that may occur due to non-internal reasons like a request
missing some parameters or is otherwise malformed.
To allow an easy handling of these type of errors the 
[`wisdomMiddleware.WISdoMErrorHandler`](https://pkg.go.dev/github.com/wisdom-oss/microservice-middlewares/v2#WISdoMErrorHandler)
inserts two channels into the requests context.

To use an error with the middleware, it needs to be declared in the 
`resources/errors.json` file.
This file contains an JSON array containing single error entries. A single error
entry looks like this:
```json
{
    "code": "VALUE_SMALLER_THAN_ZERO",
    "title": "Value smaller than zero",
    "description": "the value supplied is lower than zero.",
    "httpCode": 400
}
```
> ⚠️ Important Convention <br>
> The `code` is always to be written in SCREAMING_SNAKE_CASE <br>

The way to retrieve the two channels is analogue to the native error handling,
however, the channels have different names to allow the usage of both handlers
at the same time. Furthermore, the type of the channel into which the error has
been put in the previous section, has changed from `error` to `string`.

After getting the channels the procedure to handle an error that has previously
been defined is the same as when handling a native error, **however** this time
the code of the error is sent into the channel.
```go
func MyHandler(w http.ResponseWriter, r *http.Request) {
    // get the error channel for custom errors
    wisdomErrorHandler := r.Context().Value("wisdomErrorChannel").(chan string)
    // get the channel that will indicate that an error has been handled
    wisdomErrorHandled := r.Context().Value("wisdomErrorHandled").(chan bool)

    // your code will go after these lines
    value := thisFunctionReturnsAFloatValue()
    // now test if the function really returned an error
    if value < 0 {
        // 
        wisdomErrorHandler <- "VALUE_SMALLER_THAN_ZERO"

        // now listen on the other channel to wait for the error to be handled
        // this blocks the execution of further code until the error has been handled
        <-wisdomErrorHandled

        // since the error has been handled, exit the handler
        return
    }
}
```
