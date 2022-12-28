# Angular Directives

Angular directives are a powerful tool for extending the HTML vocabulary of an Angular application. They allow developers to create custom elements and attributes that can be used in templates to extend the functionality of the application. In this article, we will explore the different types of directives available in Angular and how to use them with examples.

# **Types of Directives**

There are three main types of directives in Angular:

1. **Component directives**: These directives have a template and a class that defines the component's behavior. They are used to create reusable UI elements in an Angular application.
    
2. **Structural directives**: These directives alter the DOM layout by adding, removing, or replacing elements. They are identified by a `*` prefix in the template. Examples of structural directives include `*ngFor` and `*ngIf`.
    
3. **Attribute directives**: These directives alter the appearance or behavior of an element, component, or another directive. They do not have a template and are identified by an attribute on an element. Examples of attribute directives include `ngClass` and `ngStyle`.
    

### **Component directives**

Component directives are a type of directive in Angular that have a template and a class that defines the component's behavior. They are used to create reusable UI elements in an Angular application.

To create a component directive in Angular, you will need to use the `@Component` decorator and define the component's selector, template, and class. For example, here is how you might create a component directive for a simple message display:

```csharp
import { Component } from '@angular/core';

@Component({
  selector: 'app-message-display',
  template: `
    <p>{{ message }}</p>
  `,
  styleUrls: ['./message-display.component.css']
})
export class MessageDisplayComponent {
  message = 'This is a message!';
}
```

You can then use your component directive in a template by including the component's selector as an element. For example:

```csharp
<app-message-display></app-message-display>
```

This would render the message "This is a message!" on the page.

You can also pass data to a component directive using input bindings. For example, you might update the component above to accept a message as an input like this:

```csharp
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-message-display',
  template: `
    <p>{{ message }}</p>
  `,
  styleUrls: ['./message-display.component.css']
})
export class MessageDisplayComponent {
  @Input() message: string;
}
```

You can then pass a message to the component like this:

```csharp
<app-message-display [message]="'Hello World!'"></app-message-display>
```

This would render the message "Hello World!" on the page.

Component directives are a powerful way to create reusable UI elements in an Angular application and can greatly simplify the development process by allowing you to encapsulate complex UI logic and markup into a single, easily-reusable component.

### Structural Directives

Structural directives are a type of directive in Angular that alter the DOM layout by adding, removing, or replacing elements. They are identified by a `*` prefix in the template.

One of the most commonly used structural directives is the `*ngFor` directive, which allows you to iterate over a list of items and render a template for each item. For example, suppose you have a list of users that you want to display in a table. You could use the `*ngFor` directive like this:

```csharp
<table>
  <tr *ngFor="let user of users">
    <td>{{ user.name }}</td>
    <td>{{ user.email }}</td>
  </tr>
</table>
```

This would render a table row for each user in the `users` list, with the user's name and email displayed in each row.

Another commonly used structural directive is the `*ngIf` directive, which allows you to conditionally render an element based on a condition. For example, you might use the `*ngIf` directive like this:

```csharp
<div *ngIf="showElement">
  This element will only be displayed if the showElement variable is truthy.
</div>
```

This would render the `div` element only if the `showElement` variable is truthy.

Structural directives are a powerful tool for altering the DOM layout in an Angular application and can greatly simplify the development process by allowing you to encapsulate complex DOM manipulation logic into a single, easily-reusable directive.

### Structural Directives

Attribute directives are a type of directive in Angular that alter the appearance or behavior of an element, component, or another directive. They do not have a template and are identified by an attribute on an element.

One of the most commonly used attribute directives is the `ngClass` directive, which allows you to add or remove CSS classes from an element based on a condition. For example, you might use the `ngClass` directive like this:

```csharp
<div [ngClass]="{ 'highlighted': highlight }">
  This element will have the 'highlighted' class applied if the highlight variable is truthy.
</div>
```

This would add the `highlighted` class to the `div` element if the `highlight` variable is truthy.

Another commonly used attribute directive is the `ngStyle` directive, which allows you to set inline styles on an element based on a condition. For example, you might use the `ngStyle` directive like this:

```csharp
<div [ngStyle]="{ 'font-size': fontSize + 'px', 'color': fontColor }">
  This element's font size and color will be set based on the fontSize and fontColor variables.
</div>
```

This would set the `font-size` and `color` styles on the `div` element based on the values of the `fontSize` and `fontColor` variables.

Attribute directives are a useful tool for altering the appearance or behavior of elements in an Angular application and can greatly simplify the development process by allowing you to encapsulate complex element manipulation logic into a single, easily-reusable directive.

# **Creating a Custom Directive**

It is also possible to create your own custom directives in Angular. To do so, you will need to use the `@Directive` decorator and define the directive's selector. For example, here is how you might create a custom attribute directive that sets the `disabled` attribute on an element:

```csharp
import { Directive, ElementRef, Input } from '@angular/core';

@Directive({
  selector: '[appDisable]'
})
export class DisableDirective {
  @Input() set appDisable(condition: boolean) {
    this.el.nativeElement.disabled = condition;
  }

  constructor(private el: ElementRef) {}
}
```

You can then use your custom directive in a template like any other directive, by including it as an attribute on an element:

```csharp
<button appDisable="true">This button is disabled</button>
```

# **Conclusion**

Directives are a powerful feature of Angular that allow developers to extend the HTML vocabulary of their applications and create custom UI elements and behaviors. By understanding the different types of directives and how to use them, you can take your Angular development skills