# Component Input Properties in Angular

In Angular, you can use component input properties to pass data from a parent component to a child component. Input properties allow you to bind values from the parent component to the child component, so the child component can display or use the data.

## **Declaring Input Properties**

To declare an input property in a child component, you can use the `@Input()` decorator. The `@Input()` decorator is a built-in decorator provided by Angular that allows you to bind a value from the parent component to the child component.

Here is an example of how to declare an input property in a child component:

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <p>{{ message }}</p>
  `
})
export class ChildComponent {
  @Input() message: string;
}
```

In this example, the `ChildComponent` has an input property called `message` of type `string`. The `@Input()` decorator is used to declare the `message` property as an input property.

## **Binding to Input Properties**

To bind a value from the parent component to the child component, you can use the `[property]` syntax in the template of the parent component. The `[property]` syntax is used to bind a value to an input property of a component.

Here is an example of how to bind a value to the `message` input property of the `ChildComponent`:

```xml
<app-child [message]="'Hello from the parent component'"></app-child>
```

In this example, the `[message]` attribute is used to bind the value `'Hello from the parent component'` to the `message` input property of the `ChildComponent`. The value of the `message` input property will be displayed in the template of the `ChildComponent`.

You can also bind a value from a property or expression in the parent component to the input property of the child component. For example:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child [message]="greeting"></app-child>
  `
})
export class ParentComponent {
  greeting = 'Hello from the parent component';
}
```

In this example, the `ParentComponent` has a property called `greeting` with the value `'Hello from the parent component'`. The `greeting` property is bound to the `message` input property of the `ChildComponent` using the `[message]` attribute.

## **Using Input Properties in the Child Component**

In the child component, you can use the input property like any other property. You can display the value of the input property in the template, or you can use it in the component class to perform logic or calculations.

Here is an example of how to use the `message` input property in the `ChildComponent`:

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <p>{{ message }}</p>
  `
})
export class ChildComponent {
  @Input() message: string;

  ngOnInit() {
    console.log(this.message);
  }
}
```

In this example, the `ChildComponent` displays the value of the `message` input property in the template using interpolation. The component also logs the value of the `message` input property to the console in the `ngOnInit()` lifecycle hook.

## **Input Property Changes**

Input properties can change if the value of the bound property or expression in the parent component changes. Angular updates the input properties of the child component whenever the bound values change.

Here is an example of how to update the value of the `message` input property in the `ParentComponent`:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child [message]="greeting"></app-child>
    <button (click)="changeGreeting()">Change Greeting</button>
  `
})
export class ParentComponent {
  greeting = 'Hello from the parent component';

  changeGreeting() {
    this.greeting = 'Hello from the changed greeting';
  }
}
```

In this example, the `ParentComponent` has a button that, when clicked, changes the value of the `greeting` property to `'Hello from the changed greeting'`. The `greeting` property is bound to the `message` input property of the `ChildComponent`, so the value of the `message` input property will also change.

You can use the `ngOnChanges()` lifecycle hook in the child component to detect changes to input properties and perform actions in response to the changes.

```typescript
import { Component, Input, OnChanges } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <p>{{ message }}</p>
  `
})
export class ChildComponent implements OnChanges {
  @Input() message: string;

  ngOnChanges(changes: SimpleChanges) {
    console.log(changes);
  }
}
```

In this example, the `ChildComponent` implements the `OnChanges` interface and has an `ngOnChanges()` method that logs the changes to the input properties to the console. The `ngOnChanges()` method is called whenever the input properties of the component change.

These are just a few examples of how to use component input properties in Angular. You can use input properties to pass data between components and update the data when it changes.

## Real-World Examples

### Customer management

Imagine you are building a customer management application for a small business. The application has a component called `CustomerDetailComponent` that displays detailed information about a customer.

The `CustomerDetailComponent` has an input property called `customer` that holds the customer object to be displayed. The `customer` object has properties for the customer's name, email, phone number, and address.

```typescript
Copy codeimport { Component, Input } from '@angular/core';

@Component({
  selector: 'app-customer-detail',
  template: `
    <h2>Customer Details</h2>
    <p>Name: {{ customer.name }}</p>
    <p>Email: {{ customer.email }}</p>
    <p>Phone: {{ customer.phone }}</p>
    <p>Address: {{ customer.address }}</p>
  `
})
export class CustomerDetailComponent {
  @Input() customer: any;
}
```

To display the `CustomerDetailComponent` in another component, you can bind the `customer` input property to a customer object in the parent component.

```xml
<app-customer-detail [customer]="selectedCustomer"></app-customer-detail>
```

In this example, the `selectedCustomer` object is bound to the `customer` input property of the `CustomerDetailComponent`. The `CustomerDetailComponent` will display the details of the `selectedCustomer`.

You can use the `*ngIf` directive to show or hide the `CustomerDetailComponent` based on the value of the `selectedCustomer` object.

```xml
<app-customer-detail *ngIf="selectedCustomer" [customer]="selectedCustomer"></app-customer-detail>
```

In this example, the `CustomerDetailComponent` is only displayed if the `selectedCustomer` object is truthy. This allows you to show or hide the component based on whether a customer is selected.

### Task management

Imagine you are building a task management application that has a component called `TaskListComponent` that displays a list of tasks. The `TaskListComponent` has an input property called `tasks` that holds the list of tasks to be displayed.

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-task-list',
  template: `
    <ul>
      <li *ngFor="let task of tasks">
        <input type="checkbox" [checked]="task.completed">
        {{ task.title }}
      </li>
    </ul>
  `
})
export class TaskListComponent {
  @Input() tasks: any[];
}
```

To display the `TaskListComponent` in another component, you can bind the `tasks` input property to a list of tasks in the parent component.

```xml
<app-task-list [tasks]="tasks"></app-task-list>
```

In this example, the `tasks` array is bound to the `tasks` input property of the `TaskListComponent`. The `TaskListComponent` will display a list of tasks with a checkbox next to each task that is checked if the task is completed.

You can use the `*ngIf` directive to show or hide the `TaskListComponent` based on the value of the `tasks` array.

```xml
<app-task-list *ngIf="tasks.length > 0" [tasks]="tasks"></app-task-list>
<p *ngIf="tasks.length == 0">No tasks to display</p>
```

In this example, the `TaskListComponent` is only displayed if the `tasks` array has a length greater than 0. If the `tasks` array is empty, a `p` element is displayed with the message "No tasks to display".

You can also use component input properties to pass data between nested components. For example, you can have a parent component that passes data to a child component, which then passes data to a grandchild component.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child [data]="parentData"></app-child>
  `
})
export class ParentComponent {
  parentData = { message: 'Hello from the parent component' };
}
```

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <app-grandchild [data]="childData"></app-grandchild>
  `
})
export class ChildComponent {
  @Input() data: any;

  get childData() {
    return { message: this.data.message };
  }
}
```

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-grandchild',
  template: `
    <p>{{ data.message }}</p>
  `
})
export class GrandchildComponent {
  @Input() data: any;
}
```