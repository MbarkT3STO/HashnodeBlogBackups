# Angular ngIf Directive

The ngIf directive is a powerful tool in the Angular template syntax that allows you to conditionally render a portion of the template based on a boolean expression. This can be particularly useful when working with data that may not always be present or when you want to display different content depending on the state of your application.

Here is a basic example of how to use the ngIf directive:

```typescript
<div *ngIf="isLoggedIn">
  Welcome, user!
</div>
```

In this example, the `div` element and its contents will only be rendered if the `isLoggedIn` variable is `true`. If it is `false`, the element will not be included in the rendered output.

You can also use the `else` clause to specify content to be rendered if the condition is `false`. For example:

```typescript
<div *ngIf="isLoggedIn; else loggedOutContent">
  Welcome, user!
</div>

<ng-template #loggedOutContent>
  Please log in to continue.
</ng-template>
```

In this case, the `div` element will be rendered if `isLoggedIn` is `true`, and the `ng-template` element (which is not rendered directly but can be used as a template reference) will be rendered if `isLoggedIn` is `false`.

## **Advanced Use of ngIf**

The ngIf directive can also be used in conjunction with an `as` clause to bind the result of the condition to a local template variable. This can be useful when you need to access the result of the condition in the template.

For example:

```typescript
<div *ngIf="user; else noUser">
  Welcome, {{ user.name }}!
</div>

<ng-template #noUser>
  Please log in to continue.
</ng-template>
```

In this case, the `div` element will be rendered if `user` is truthy (e.g. an object with a name property), and the `ng-template` element will be rendered if `user` is falsy (e.g. `null` or `undefined`). The `user` variable can then be accessed within the template using the `user` template variable.

## **Conclusion**

The ngIf directive is a valuable tool for controlling the rendering of elements in an Angular template based on boolean conditions. It allows you to create dynamic templates that can adapt to different data and application states. By using the `else` clause and the `as` clause, you can further customize the behavior of ngIf to fit your specific needs.