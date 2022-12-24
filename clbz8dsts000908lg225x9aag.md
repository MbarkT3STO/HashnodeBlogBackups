# Component Output Properties in Angular

## **Introduction**

Component output properties allow a component to communicate with its parent component in Angular applications. This is done through the use of event bindings and custom events.

## **Event Bindings**

Event bindings allow a component to bind to DOM events emitted by its template. These events can be triggered by user actions, such as clicks or keystrokes, or by the component itself through programming logic.

To bind to an event, you can use the `(event)` syntax in the template, where `event` is the name of the DOM event you want to bind to. For example, to bind to the `click` event, you can use the following syntax:

```xml
<button (click)="onClick()">Click me</button>
```

In the component class, you can then define the `onClick()` method to handle the event:

```typescript
export class MyComponent {
  onClick() {
    // Handle the click event here
  }
}
```

## **Custom Events**

In addition to binding to DOM events, a component can also create and emit its own custom events. This allows the component to communicate with its parent component or with other components in the application.

To create a custom event, you can use the `@Output` decorator in the component class and bind to it in the template using the `(event)` syntax. For example:

```typescript
import { Output, EventEmitter } from '@angular/core';

export class MyComponent {
  @Output() myEvent = new EventEmitter<string>();

  emitEvent() {
    this.myEvent.emit('some value');
  }
}
```

In the template, you can bind to the custom event using the `(event)` syntax:

```xml
<button (click)="emitEvent()">Emit Event</button>
<div (myEvent)="onMyEvent($event)">Event will be emitted here</div>
```

In the component class, you can define the `onMyEvent()` method to handle the custom event:

```typescript
export class MyComponent {
  // Other component code

  onMyEvent(value: string) {
    // Handle the custom event here
  }
}
```

## Example

One real-world example of using component output properties is in a form submission process. Suppose you have a form component that collects user information, such as a login form or a contact form. The form component could have a `submit` event binding to handle the submission process and a `success` custom event to notify the parent component that the form was submitted successfully.

Here is an example of how this could be implemented in the form component class:

```typescript
import { Output, EventEmitter } from '@angular/core';

export class FormComponent {
  @Output() success = new EventEmitter<void>();

  onSubmit() {
    // Validate and submit the form here
    this.success.emit();
  }
}
```

In the template, you can bind to the `submit` event and the `success` custom event:

```xml
<form (submit)="onSubmit()">
  <!-- Form fields and buttons go here -->
</form>
```

Then, in the parent component, you can bind to the `success` event and display a success message or redirect to another page:

```xml
<app-form (success)="onSuccess()">
  <!-- Form content goes here -->
</app-form>
```

```typescript
export class ParentComponent {
  onSuccess() {
    // Display a success message or redirect to another page
  }
}
```

This way, the form component can handle its own submission process and notify the parent component when it is completed, allowing the parent component to respond accordingly.

**Another example of using component output properties is in a pagination** component. Suppose you have a pagination component that displays a list of page numbers and allows the user to navigate through the pages. The pagination component could have a `pageChange` custom event to notify the parent component when the current page changes.

Here is an example of how this could be implemented in the pagination component class:

```typescript
import { Output, EventEmitter } from '@angular/core';

export class PaginationComponent {
  @Output() pageChange = new EventEmitter<number>();
  currentPage = 1;

  onPageChange(page: number) {
    this.currentPage = page;
    this.pageChange.emit(page);
  }
}
```

In the template, you can bind to the `click` event of the page numbers and call the `onPageChange()` method with the page number as an argument:

```xml
<ul>
  <li *ngFor="let page of pages" (click)="onPageChange(page)">{{ page }}</li>
</ul>
```

Then, in the parent component, you can bind to the `pageChange` event and update the data displayed on the page based on the current page number:

```xml
<app-pagination (pageChange)="onPageChange($event)">
  <!-- Pagination content goes here -->
</app-pagination>
```

```typescript
export class ParentComponent {
  onPageChange(page: number) {
    // Load the data for the current page and update the display
  }
}
```

This way, the pagination component can handle the page navigation and notify the parent component when the current page changes, allowing the parent component to update the displayed data accordingly.

## **Conclusion**

Component output properties are a powerful way for a component to communicate with its parent component or with other components in an Angular application. By using event bindings and custom events, you can create dynamic and interactive applications that respond to user actions and internal logic.