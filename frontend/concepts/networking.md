# Networking

In this document, we'll discuss how to use the HTTP client and built-in 
interceptors for making network requests in the WISdoM Frontend.

## HttpClient

For networking, one should always use the 
[HttpClient](https://angular.io/api/common/http/HttpClient) provided by Angular. 
Requests made through this client are intercepted by the interceptors that 
modify network traffic passing through the application.

## Interceptors

The core consists of several interceptors that modify the requests and responses 
passing through the application:

- `AuthInterceptor`: 
  This interceptor injects the authorization header to routes demanding it.

- `BaseUrlInterceptor`: 
  This interceptor injects the base URL or API URL if requested.

- `CacheInterceptor`: 
  This interceptor injects headers for caching.

- `ErrorInterceptor`: 
  This interceptor either displays a big curtain or a toast notification if the 
  request failed with a 5XX or 4XX.

- `LoaderInterceptor`: 
  This interceptor enables displaying a loader curtain for long-lasting 
  requests.

- `SanitizeUrlInterceptor`: 
  This interceptor sanitizes the URL, for now it only removes double slashes, 
  where only one slash should be.

Externally injecting interceptors is currently not needed and therefore not 
integrated.

## HttpContext

To give instructions to the interceptors, the 
[HttpContext](https://angular.io/api/common/http/HttpContext) class is used. 
All the contexts are defined under `wisdom_modules/common/src/lib/http-context`.

- `SEND_AUTH`: 
  Defaults to `undefined`. 
  If `true`, the `AuthInterceptor` will add the `Authorization` header, not 
  needed if `USE_API_URL` is already set to `true`.

- `USE_API_URL`: 
  Defaults to `false`. 
  If `true`, the `AuthInterceptor` will inject the `Authorization` header, 
  except `SEND_AUTH` is set to `false`.
  
- `USE_BASE_URL`: 
  Defaults to `false`. 
  If `true`, the `BaseUrlInterceptor` will insert the base URL defined in the 
  environment.
  
- `USE_CACHE`: 
  Defaults to `true`. 
  If `true`, the `CacheInterceptor` will inject the necessary caching headers. 
  Disable this to disable caching.
  
- `USE_ERROR_HANDLER`: 
  Defaults to `USE_ERRO_HANDLER.handler.CURTAIN`.
  Uses the enum `ErrorHandler` to decide what should happen on an error.
  If `CURTAIN`, raise a curtain displaying the error message and covering the 
  entire main container.
  If `TOAST`, display a small toast notification showing the error and allowing 
  continued use of the main container. 
  If `CUSTOM`, the implementor should handle the error.
  
- `USE_LOADER`: 
  Defaults to `false`. 
  If `true`, the `LoaderInterceptor` will prompt a loading screen until the 
  request responds. 
  If the content is a string, it will be used to render a loading message. 
  This can also be translated, useful for long loading requests.

By using these contexts, you can instruct the interceptors what they should do 
clearly without having to tamper with the interceptors yourself.

## Example

Here's an example of how to use these contexts in a service:

```ts
import { Injectable } from '@angular/core';
import {HttpClient, HttpContext, HttpHeaders} from "@angular/common/http";
import {Observable} from "rxjs";
import {Router} from "@angular/router";
import {USE_API_URL, USE_LOADER} from "common";

const API_URL = "prophet-forecast/";

// some types...

@Injectable({
  providedIn: 'root'
})
export class ProphetForecastService {

  // the typical dependency injections for services requesting data
  constructor(
    private http: HttpClient,
    private router: Router
  ) {}

  // a fetcher using the mentioned contexts
  fetchForecastData(key: string): Observable<ForecastResponse> {
    let url = this.router.parseUrl(API_URL);
    url.queryParams["key"] = key;
    return this.http.get(url.toString(), {
      headers: new HttpHeaders({
        "Content-Type": "application/json"
      }),
      responseType: "json",
      context: new HttpContext()
        .set(USE_API_URL, true)
        .set(USE_LOADER, "water-usage-forecasts.display.loading")
    }) as Observable<ForecastResponse>;
  }
}
```

The `fetchForecastData` method of this service is a great example of how 
networking should be done:

1. First, construct a request, most often using the `Router` and a constant 
   `API_URL` you defined at the top.

2. Set all headers and contexts. 
   Here, the API will be requested, so the `USE_API_URL` is enabled. 
   Also, this request takes some time, therefore `USE_LOADER` is enabled with a 
   string that gets translated later.

3. Often the return type cannot be correctly inferred, therefore some correction 
  may be done via the `as Observable<...>` statement.
