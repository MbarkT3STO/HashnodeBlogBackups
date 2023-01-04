# Angular ngStyle Directive

The ngStyle directive is a built-in Angular directive that lets you set the style of an element dynamically. This can be useful when you want to change the style of an element based on some condition or data.

## **How to use the ngStyle Directive**

To use the ngStyle directive, you need to bind it to an element using the following syntax:

```xml
<element [ngStyle]="expression">
```

Here, `expression` is an Angular expression that returns an object with style properties and their values. For example:

```xml
<div [ngStyle]="{'font-size': '20px', 'color': 'red'}">
  This text will be red and 20px in size.
</div>
```

You can also bind the ngStyle directive to a component property and use it to set the style of the element. For example:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div [ngStyle]="style">
      This text will have the style defined in the component.
    </div>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  style = {
    'font-size': '20px',
    'color': 'red'
  };
}
```

## **Examples**

Here are a few examples of how you can use the ngStyle directive in your Angular application:

### **Example 1: Changing the style based on a condition**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div [ngStyle]="{'font-size': fontSize, 'color': color}">
      This text will have the style defined in the component.
    </div>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  fontSize = '20px';
  color = 'red';

  constructor() {
    setTimeout(() => {
      this.fontSize = '30px';
      this.color = 'blue';
    }, 5000);
  }
}
```

In this example, the style of the div element will change after 5 seconds to a font size of 30px and a color of blue.

### **Example 2: Changing the style based on a component input**

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-style-demo',
  template: `
    <div [ngStyle]="{'font-size': fontSize, 'color': color}">
      This text will have the style defined in the component.
    </div>
  `,
  styleUrls: ['./app.component.css']
})
export class StyleDemoComponent {
  @Input() fontSize: string;
  @Input() color: string;
}
```

In this example, the style of the div element can be controlled by the parent component using the `fontSize` and `color` inputs. The parent component can set the style by passing the desired values to the inputs like this:

```typescript
<app-style-demo [fontSize]="'20px'" [color]="'red'"></app-style-demo>
```

This would set the font size of the div element to 20px and the color to red.

### **Example 3: Using ngStyle with a component method**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <div [ngStyle]="style">
      This text will have the style defined in the component.
    </div>
    <button (click)="changeStyle()">Change Style</button>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  style = {
    'font-size': '20px',
    'color': 'red'
  };

  changeStyle() {
    this.style = {
      'font-size': '30px',
      'color': 'blue'
    };
  }
}
```

In this example, the style of the div element can be changed by clicking the button. When the button is clicked, the `changeStyle()` method is called and the style of the div element is updated to a font size of 30px and a color of blue.

## **Conclusion**

The ngStyle directive is a powerful tool that allows you to set the style of an element dynamically in your Angular application. Whether you want to change the style based on a condition, component input, or a method call, the ngStyle directive makes it easy to do so.