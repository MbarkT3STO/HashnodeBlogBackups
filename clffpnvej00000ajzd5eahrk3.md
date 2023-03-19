---
title: "Angular: What is Interceptor and How to Use it"
datePublished: Sun Mar 19 2023 18:07:57 GMT+0000 (Coordinated Universal Time)
cuid: clffpnvej00000ajzd5eahrk3
slug: angular-what-is-interceptor-and-how-to-use-it
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679245428058/db7411ea-6377-41e3-b3b9-b825cbb5af49.png
tags: angularjs, net, aspnet-core

---

Angular Interceptor is a powerful feature in the Angular framework that allows developers to intercept and handle HTTP requests and responses. This feature enables developers to perform actions like adding headers, modifying request payloads, or handling errors before the request is sent or the response is received. This article will provide a brief overview of Angular Interceptor and how to use it with examples.

## **What is Angular Interceptor?**

In Angular, Interceptor is a middleware that sits between the HTTP request and the response. It allows developers to intercept HTTP requests and responses and perform actions like adding headers, modifying request payloads, or handling errors before the request is sent or the response is received.

Interceptor provides a way to modify requests and responses at a centralized location rather than making changes in multiple locations. This feature can be used for various purposes like authentication, caching, logging, or handling errors.

## **How to Use Angular Interceptor?**

To use Angular Interceptor, we need to create an Interceptor class that implements the HttpInterceptor interface. This interface provides two methods - `intercept()` and `handleError()`. The `intercept()` method is used to intercept HTTP requests and responses, while the `handleError()` method is used to handle errors that occur during the HTTP request.

```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Add Authorization header to the request
    const token = localStorage.getItem('token');
    if (token) {
      req = req.clone({
        setHeaders: {
          Authorization: `Bearer ${token}`
        }
      });
    }
    return next.handle(req).pipe(
      catchError(err => {
        // Handle errors here
        return throwError(err);
      })
    );
  }

}
```

In the above example, we created an Interceptor class `AuthInterceptor` that implements the `HttpInterceptor` interface. In the `intercept()` method, we are adding an Authorization header to the HTTP request if the token is available in the localStorage. We are also using the `catchError()` operator to handle errors that occur during the HTTP request.

To use the Interceptor, we need to provide it in the `providers` array of the `NgModule`. For example:

```typescript
import { NgModule } from '@angular/core';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './auth.interceptor';

@NgModule({
  imports: [
    HttpClientModule
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true
    }
  ]
})
export class AppModule { }
```

In the above example, we provided the `AuthInterceptor` in the `providers` array of the `NgModule`. We also used the `HTTP_INTERCEPTORS` token to specify that this provider is an Interceptor.

## **Conclusion**

Angular Interceptor is a powerful feature that allows developers to intercept and handle HTTP requests and responses. This feature provides a way to modify requests and responses at a centralized location rather than making changes in multiple locations. In this article, we provided a brief overview of Angular Interceptor and how to use it with examples.