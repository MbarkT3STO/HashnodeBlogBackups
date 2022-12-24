# How to Display a List of Elements in an Angular Component

In Angular, you can use the `*ngFor` directive to display a list of elements in the HTML template of a component. The `*ngFor` directive is a built-in structural directive that iterates over a list of items and renders a template for each item.

Here is an example of how to use the `*ngFor` directive to display a list of elements in an Angular component:

## **Creating the Component Class**

First, create a component class with a property that contains the list of elements you want to display. For example:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-my-component',
  template: `
    <ul>
      <li *ngFor="let element of elements">{{ element }}</li>
    </ul>
  `
})
export class MyComponent {
  elements = ['Element 1', 'Element 2', 'Element 3'];
}
```

In this example, the `MyComponent` has a property called `elements` with an array of strings. The `elements` array will be used to display a list of elements in the component's template.

## **Using the** `*ngFor` **Directive in the Template**

To display the list of elements in the component's template, you can use the `*ngFor` directive in a template element. The `*ngFor` directive takes a list of items and a template to render for each item.

In the template, you can use the `*ngFor` directive with the `let` keyword to bind a template element to each item in the list. For example:

```xml
<ul>
  <li *ngFor="let element of elements">{{ element }}</li>
</ul>
```

## Examples

### To-do list application

Imagine you are building a to-do list application. The application has a component called `TaskListComponent` that displays a list of tasks.

You can create the `TaskListComponent` class with a property called `tasks` that contains the list of tasks:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-task-list',
  template: `
    <ul>
      <li *ngFor="let task of tasks">{{ task.title }}</li>
    </ul>
  `
})
export class TaskListComponent {
  tasks = [
    { title: 'Task 1', completed: false },
    { title: 'Task 2', completed: false },
    { title: 'Task 3', completed: true }
  ];
}
```

In this example, the `TaskListComponent` has a property called `tasks` with an array of task objects. Each task object has a `title` and a `completed` property.

To display the list of tasks in the component's template, you can use the `*ngFor` directive in a `li` element:

```xml
<ul>
  <li *ngFor="let task of tasks">{{ task.title }}</li>
</ul>
```

This will display a list of tasks with the task titles in an unordered list.

You can use the `*ngFor` directive to display more complex elements in the list. For example, you can use the `*ngFor` directive to display a checkbox next to each task to indicate whether the task is completed:

```xml
<ul>
  <li *ngFor="let task of tasks">
    <input type="checkbox" [checked]="task.completed">
    {{ task.title }}
  </li>
</ul>
```

In this example, the `*ngFor` directive is used to iterate over the `tasks` array and render a `li` element for each task. The `li` element has an input element with a `type` attribute set to `"checkbox"` and a `[checked]` attribute set to the value of the `task.completed` property. This will display a checkbox next to each task that is checked if the task is completed.

You can also use the `*ngFor` directive to apply styles to the list items based on the values of the task properties. For example, you can use the `*ngFor` directive to apply a class to the list items that are completed:

```xml
<ul>
  <li *ngFor="let task of tasks" [class.completed]="task.completed">
    <input type="checkbox" [checked]="task.completed">
    {{ task.title }}
  </li>
</ul>
```

In this example, the `*ngFor` directive is used to iterate over the `tasks` array and render a `li` element for each task. The `li` element has a `[class.completed]` attribute set to the value of the `task.completed` property. This will add the `completed` class to the `li` element if the task is completed. You can then use CSS to style the completed tasks differently.

### Product Catalog

Imagine you are building a product catalog for an online store. The catalog has a component called `ProductListComponent` that displays a list of products.

You can create the `ProductListComponent` class with a property called `products` that contains the list of products:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-product-list',
  template: `
    <div *ngFor="let product of products">
      <h3>{{ product.name }}</h3>
      <p>{{ product.description }}</p>
      <p>{{ product.price | currency }}</p>
      <button (click)="addToCart(product)">Add to Cart</button>
    </div>
  `
})
export class ProductListComponent {
  products = [
    { name: 'Product 1', description: 'Description 1', price: 19.99 },
    { name: 'Product 2', description: 'Description 2', price: 29.99 },
    { name: 'Product 3', description: 'Description 3', price: 39.99 }
  ];

  addToCart(product: any) {
    // Add the product to the shopping cart
  }
}
```

In this example, the `ProductListComponent` has a property called `products` with an array of product objects. Each product object has a `name`, `description`, and `price` property.

To display the list of products in the component's template, you can use the `*ngFor` directive in a `div` element:

```xml
<div *ngFor="let product of products">
  <h3>{{ product.name }}</h3>
  <p>{{ product.description }}</p>
  <p>{{ product.price | currency }}</p>
  <button (click)="addToCart(product)">Add to Cart</button>
</div>
```

In this example, the `*ngFor` directive is used to iterate over the `products` array and render a `div` element for each product. The `div` element contains the `name`, `description`, and `price` of the product, as well as a button to add the product to the shopping cart.

The `addToCart()` method is called when the button is clicked. The method takes the product object as an argument and adds it to the shopping cart.

You can use the `*ngFor` directive to display more complex elements in the list. For example, you can use the `*ngFor` directive to display an image for each product:

```xml
<div *ngFor="let product of products">
  <img [src]="product.imageUrl">
  <h3>{{ product.name }}</h3>
  <p>{{ product.description }}</p>
  <p>{{ product.price | currency }}</p>
  <button (click)="addToCart(product)">Add to Cart</button>
</div>
```

In this example, the `*ngFor` directive is used to iterate over the `products` array and render a `div` element for each product. The `div` element contains an `img` element with a `[src]` attribute set to the value of the `product.imageUrl` property. This will display the image of the product.