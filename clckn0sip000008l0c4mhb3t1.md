# Routing in Angular

Routing in Angular allows users to navigate between different parts of a web application, or between different web applications, as they click on links or perform other actions. Angular's router enables developers to set up multiple routes, each with its own component, and navigate between them based on user actions.

## **Setting up the Router**

To use the router in an Angular application, you need to first import the router module and add it to the `imports` array in the `@NgModule` decorator.

```typescript
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  { path: 'path1', component: Component1 },
  { path: 'path2', component: Component2 },
  { path: '', redirectTo: '/path1', pathMatch: 'full' },
  { path: '**', component: PageNotFoundComponent }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

In the above code, we have defined an array of routes, each with a path and a component. The `path` property specifies the URL that the route corresponds to, and the `component` property specifies the component that should be displayed when the route is activated.

The `''` route is a default route that redirects to `'/path1'` if the URL is empty. The `'**'` route is a wildcard route that is used to display a "page not found" component if the user navigates to a URL that doesn't match any of the defined routes.

## **Navigating between Routes**

Once you have set up the routes in your application, you can navigate between them using the `routerLink` directive. This directive can be used with an anchor tag to create a link to a specific route.

```xml
<a [routerLink]="['/path1']">Go to path1</a>
<a [routerLink]="['/path2']">Go to path2</a>
```

You can also navigate to a route programmatically using the `Router` service. To do this, you need to inject the `Router` service into your component and call the `navigate` method with the desired route path.

```typescript
import { Router } from '@angular/router';

constructor(private router: Router) {}

goToPath1() {
  this.router.navigate(['/path1']);
}
```

## **Passing Parameters to Routes**

Sometimes you may want to pass parameters to a route, such as an ID or query string. You can do this by adding a parameter to the route path and using the `ActivatedRoute` service to access the parameter in the component.

```typescript
const routes: Routes = [
  { path: 'path/:id', component: Component }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

```typescript
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) {}

ngOnInit() {
  this.id = this.route.snapshot.paramMap.get('id');
}
```

In the above code, we are injecting the `ActivatedRoute` service into the component and using it to access the `id` parameter from the route path. The `snapshot.paramMap` property returns an object containing the route parameters, and the `get` method can be used to retrieve a specific parameter by its name.

You can also pass query parameters to a route by using the `queryParams` property of the `routerLink` directive or the `navigate` method.

```xml
<a [routerLink]="['/path']" [queryParams]="{ param: value }">Link with query params</a>

this.router.navigate(['/path'], { queryParams: { param: value } });
```

## **Protecting Routes with Guards**

Sometimes you may want to restrict access to certain routes based on certain conditions. For example, you may want to prevent users from accessing a route unless they are authenticated. You can use route guards to implement this behavior in Angular.

A route guard is a service that implements the `CanActivate` interface and returns a boolean value indicating whether the route can be activated or not. To use a route guard, you need to include it in the `canActivate` property of the route configuration.

```typescript
const routes: Routes = [
  { path: 'path', component: Component, canActivate: [AuthGuard] }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

```typescript
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Injectable } from '@angular/core';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    return this.authService.isAuthenticated();
  }
}
```

In the above code, we have defined a route guard called `AuthGuard` that implements the `canActivate` method. This method is called by the router whenever the route is activated, and it returns a boolean value indicating whether the route can be accessed or not. In this case, we are using the `authService` to check if the user is authenticated.

## **Conclusion**

Routing is an important aspect of web development, and Angular provides a powerful and flexible router to help you manage navigation in your applications. By setting up routes and using the `routerLink` directive or the `Router` service, you can easily navigate between different parts of your application and pass parameters to routes. You can also use route guards to protect routes and restrict access based on certain conditions.