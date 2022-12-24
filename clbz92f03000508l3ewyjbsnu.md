# EventEmitter in Angular

## **Introduction**

The `EventEmitter` class is a part of the Angular core library that allows a component to communicate with its parent component or with other components in an Angular application. It is commonly used in conjunction with the `@Output` decorator to create custom events that a component can emit.

## **Creating an EventEmitter**

To create an `EventEmitter`, you first need to import it from the Angular core library:

```typescript
import { EventEmitter } from '@angular/core';
```

Then, you can create a new `EventEmitter` instance in your component class by using the `new` keyword and passing in the type of data that the event will emit as a generic type argument:

```typescript
export class MyComponent {
  myEvent = new EventEmitter<string>();
}
```

This creates a new `EventEmitter` instance called `myEvent` that will emit values of type `string`.

# **Introduction to the EventEmitter in Angular**

The `EventEmitter` class is a part of the Angular core library that allows a component to communicate with its parent component or with other components in an Angular application. It is commonly used in conjunction with the `@Output` decorator to create custom events that a component can emit.

## **Creating an EventEmitter**

To create an `EventEmitter`, you first need to import it from the Angular core library:

```typescript
import { EventEmitter } from '@angular/core';
```

Then, you can create a new `EventEmitter` instance in your component class by using the `new` keyword and passing in the type of data that the event will emit as a generic type argument:

```typescript
export class MyComponent {
  myEvent = new EventEmitter<string>();
}
```

This creates a new `EventEmitter` instance called `myEvent` that will emit values of type `string`.

## **Using the EventEmitter**

To use the `EventEmitter`, you can bind to it in the template using the `(event)` syntax and define a corresponding method in the component class to handle the event.

For example, in the template you can bind to the `myEvent` event like this:

```xml
<div (myEvent)="onMyEvent($event)">Event will be emitted here</div>
```

Then, in the component class, you can define the `onMyEvent()` method to handle the event:

```typescript
export class MyComponent {
  // Other component code

  onMyEvent(value: string) {
    // Handle the event here
  }
}
```

To emit the event from the component, you can use the `emit()` method of the `EventEmitter` instance. For example:

```typescript
export class MyComponent {
  // Other component code

  emitEvent() {
    this.myEvent.emit('some value');
  }
}
```

You can then call the `emitEvent()` method from the template or from within the component class to emit the `myEvent` event.

## **Example: Form Submission**

One common use case for the `EventEmitter` is in a form submission process. Suppose you have a form component that collects user information, such as a login form or a contact form. The form component could have a `submit` event binding to handle the submission process and a `success` custom event to notify the parent component that the form was submitted successfully.

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

Then, in the parent component, you can bind to the `success` event and display a success message or redirect to another page.