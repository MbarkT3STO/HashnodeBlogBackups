# Angular Container and Nested Components

## Introduction

In Angular, a component is a reusable piece of UI that contains a template, logic, and styles. Components can be nested inside other components, forming a tree-like structure called a component hierarchy.

A component that contains other components is called a container component, while the components it contains are called nested components. Container components and nested components work together to build complex UI elements and layouts.

## **Creating Container and Nested Components**

To create a container component in Angular, you can create a component class and a template that contains other components. For example:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-container',
  template: `
    <app-nested1></app-nested1>
    <app-nested2></app-nested2>
  `
})
export class ContainerComponent { }
```

In this example, the `ContainerComponent` is a container component that contains two nested components: `Nested1Component` and `Nested2Component`. The template uses the `<app-nested1>` and `<app-nested2>` elements to include the nested components.

To create a nested component in Angular, you can create a component class and a template. For example:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-nested1',
  template: '<p>I am a nested component</p>' })
export class Nested1Component { }
```

In this example, the `Nested1Component` is a nested component that contains a simple template with a `<p>` element.

## Communication Between Container and Nested Components

There are several ways to communicate between container and nested components in Angular:

### Input/Output Properties

Input/output properties allow you to pass data from a container component to a nested component using input properties, and from a nested component to a container component using output properties. For example, a container component can pass data to a nested component using the `[inputProperty]` syntax in the template:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-container',
  template: `<app-nested1 [data]="nestedData"></app-nested1>`
})
export class ContainerComponent {
  nestedData = 'Some data';
}
```

In this example, the `ContainerComponent` has a property called `nestedData` with the value `'Some data'`. The template uses the `[data]` input property to pass the value of `nestedData` to the `Nested1Component`.

The `Nested1Component` can receive the data using an input decorator in the component class:

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-nested1',
  template: `
    <p>I am a nested component and I received data: {{ data }}</p>
  `
})
export class Nested1Component {
  @Input() data: string;
}
```

In this example, the `Nested1Component` has an input property called `data` decorated with the `@Input` decorator. The value of the `data` input property will be set by the container component. The template uses interpolation to display the value of `data`.

A nested component can pass data to a container component using an output event and the `(outputEvent)` syntax in the template:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-nested1',
  template: `
    <button (click)="sendData()">Send data</button>
  `
})
export class Nested1Component {
  @Output() dataEvent = new EventEmitter<string>();

  sendData() {
    this.dataEvent.emit('Some data');
  }
}
```

In this example, the `Nested1Component` has an output event called `dataEvent` decorated with the `@Output`

## Examples

### Customer Management

Imagine you are building a customer management system for a company. The system has a list of customers and a form to add new customers. The list of customers and the form are two different components that need to be reused in different parts of the application.

You can create a container component called `CustomersComponent` that contains the list of customers and the form:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-customers',
  template: `
    <app-customer-list></app-customer-list>
    <app-customer-form></app-customer-form>
  `
})
export class CustomersComponent { }
```

The `CustomersComponent` has a template that includes the `<app-customer-list>` and `<app-customer-form>` elements, which are the nested components.

The `CustomerListComponent` is a nested component that displays a list of customers:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-customer-list',
  template: `
    <table>
      <tr *ngFor="let customer of customers">
        <td>{{ customer.name }}</td>
        <td>{{ customer.email }}</td>
      </tr>
    </table>
  `
})
export class CustomerListComponent {
  customers = [
    { name: 'John', email: 'john@example.com' },
    { name: 'Jane', email: 'jane@example.com' }
  ];
}
```

The `CustomerListComponent` has a property called `customers` with a list of customer objects. The template uses the `*ngFor` directive to iterate over the `customers` array and display the customer names and emails in a table.

The `CustomerFormComponent` is a nested component that displays a form to add new customers:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-customer-form',
  template: `
    <form (submit)="addCustomer()">
      <label>
        Name:
        <input type="text" [(ngModel)]="name" name="name">
      </label>
      <br>
      <label>
        Email:
        <input type="email" [(ngModel)]="email" name="email">
      </label>
      <br>
      <button type="submit">Add Customer</button>
    </form>
  `
})
export class CustomerFormComponent {
  name: string;
  email: string;

  addCustomer() {
    // Add the customer to the list of customers
  }
}
```

The `CustomerFormComponent` has two properties: `name` and `email`. The template has a form with two input fields for the `name` and `email` properties, and a button to submit the form. The `addCustomer()` method is called when the form is submitted.

The `CustomersComponent` can be used in any part of the application to display the list of customers and the form to add new customers. The `CustomerListComponent` and `CustomerFormComponent` can be reused in other parts of the application as well.

For example, you can use the `CustomersComponent` in a `DashboardComponent` that displays the customers in the dashboard:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-dashboard',
  template: `
    <app-customers></app-customers>
  `
})
export class DashboardComponent { }
```

The `DashboardComponent` has a template that includes the `<app-customers>` element, which will display the list of customers and the form to add new customers.

### Shopping Cart

Here is another real-world example of using container and nested components in an Angular application.

Imagine you are building a shopping cart for an online store. The shopping cart has a list of items and a checkout form. The list of items and the checkout form are two different components that need to be reused in different parts of the application.

You can create a container component called `CartComponent` that contains the list of items and the checkout form:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-cart',
  template: `
    <app-cart-items></app-cart-items>
    <app-checkout-form></app-checkout-form>
  `
})
export class CartComponent { }
```

The `CartComponent` has a template that includes the `<app-cart-items>` and `<app-checkout-form>` elements, which are the nested components.

The `CartItemsComponent` is a nested component that displays a list of items in the cart:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-cart-items',
  template: `
    <table>
      <tr *ngFor="let item of items">
        <td>{{ item.name }}</td>
        <td>{{ item.price | currency }}</td>
        <td>{{ item.quantity }}</td>
      </tr>
    </table>
  `
})
export class CartItemsComponent {
  items = [
    { name: 'T-Shirt', price: 19.99, quantity: 2 },
    { name: 'Jeans', price: 49.99, quantity: 1 }
  ];
}
```

The `CartItemsComponent` has a property called `items` with a list of item objects. The template uses the `*ngFor` directive to iterate over the `items` array and display the item names, prices, and quantities in a table.

The `CheckoutFormComponent` is a nested component that displays a form to checkout the items in the cart:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-checkout-form',
  template: `
    <form (submit)="checkout()">
      <label>
        Name:
        <input type="text" [(ngModel)]="name" name="name">
      </label>
      <br>
      <label>
        Email:
        <input type="email" [(ngModel)]="email" name="email">
      </label>
      <br>
      <label>
        Address:
        <input type="text" [(ngModel)]="address" name="address">
      </label>
      <br>
      <button type="submit">Checkout</button>
    </form>
  `
})
export class CheckoutFormComponent {
  name: string;
  email: string;
  address: string;

  checkout() {
    // Process the checkout
  }
}
```

The `CheckoutFormComponent` has three properties: `name`, `email`, and `address`. The template has a form with input fields for the `name`, `email`, and `address` properties, and a button to submit the form. The `checkout()` method is called when the form is submitted.

The `CartComponent` can be used in any part of the application to display the list of items in the cart and the checkout form. The `CartItemsComponent` and `CheckoutFormComponent` can be reused in other parts of the application as well.

For example, you can use the `CartComponent` in a `CheckoutPageComponent` that displays the cart and checkout form in the checkout page:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-checkout-page',
  template: `
    <app-cart></app-cart>
  `
})
export class CheckoutPageComponent { }
```

The `CheckoutPageComponent` has a template that includes the `<app-cart>` element, which will display the list of items in the cart and the checkout form.