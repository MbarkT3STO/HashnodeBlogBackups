# What is Angular Event Binding?

## Introduction

Angular event binding is a way to bind a component's method to an element's event in the template. It allows you to execute a component's method in response to an element's event, such as a button click or a form submission.

Event binding is a one-way data binding technique, meaning that data flows from the template to the component, but not the other way around. It is often used to bind the template's elements to the component's methods.

## **Using Event Binding**

To use event binding, you can use the `(event)` syntax in the template and bind it to a component's method. For example:

```xml
<button (click)="doSomething()">Click me</button>
```

In this example, the `doSomething()` method of the component is bound to the `click` event of the `<button>` element. When the button is clicked, the `doSomething()` method will be executed.

You can also use event binding with event payloads, such as:

```xml
<input (keyup)="onKeyUp($event)">
```

In this example, the `onKeyUp()` method of the component is bound to the `keyup` event of the `<input>` element. When a key is released, the `onKeyUp()` method will be executed and passed the event payload as an argument.

## Examples

### Counter

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-my-component',
  template: `
    <button (click)="incrementCount()">Click me</button>
    <p>Count: {{ count }}</p>
  `
})
export class MyComponent {
  count = 0;

  incrementCount() {
    this.count++;
  }
}
```

In this example, the `MyComponent` has a property called `count` with the initial value of `0`. The template has a `<button>` element with an event binding that calls the `incrementCount()` method when clicked. The template also displays the value of `count` using interpolation.

When the component is rendered, the user can click the button to increment the value of `count`. The template will update the value of `count` in the `<p>` element using interpolation.

For example, if the user clicks the button three times, the template will display the following:

```xml
<button (click)="incrementCount()">Click me</button>
<p>Count: 3</p>
```

The `count` property will have the value of `3`, and the template will display it in the `<p>` element.

### Greeting

Using event binding to show a greeting message in an alert box in an Angular component:

```typescript

@Component({
  selector: 'app-my-component',
  template: `
    <button (click)="showGreeting()">Click me</button>
  `
})
export class MyComponent {
  name = 'MBARK';

  showGreeting() {
    alert(`Hello, ${this.name}!`);
  }
}
```

In this example, the `MyComponent` has a property called `name` with the value `'MBARK'`. The template has a `<button>` element with an event binding that calls the `showGreeting()` method when clicked.

When the component is rendered, the user can click the button to show the greeting message in an alert box. The `showGreeting()` method will use the `alert()` function to display the greeting message.

For example, if the user clicks the button, an alert box will be displayed with the following message:

```typescript
Hello, MBARK!
```

The alert message will use the value of `name`, which is `'MBARK'` in this case.

## **Event Binding and Security**

Event binding is safe to use, as it does not allow the execution of arbitrary code. It only allows the execution of component methods in response to element events.

# **Conclusion**

Angular event binding is a powerful way to bind the template's elements to the component's methods. It allows you to execute a component's method in response to an element's event using the `(event)` syntax and event payloads. Event binding is a one-way data binding technique that is safe to use, as it does not allow the execution of arbitrary code.