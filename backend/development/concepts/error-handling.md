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


## Handling a native error
After the basics gave an overview on how errors work in Go, this section will
now discuss, how do I even handle an error if a function returned one.

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

