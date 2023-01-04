# Angular Pipes

Angular pipes are a way to transform data in an Angular template. They are a simple way to show filtered or formatted data to the user. Angular comes with a set of built-in pipes, but you can also create your own custom pipes.

## **Built-in Pipes**

Here are a few examples of built-in pipes:

### **Currency Pipe**

The currency pipe formats a number as a currency string. It takes an optional parameter to specify the currency code, such as `USD` for US dollars or `EUR` for euros.

```xml
<p>{{ price | currency }}</p>
<p>{{ price | currency:'USD' }}</p>
<p>{{ price | currency:'EUR' }}</p>
```

### **Date Pipe**

The date pipe formats a date object as a string. It takes an optional parameter to specify the format, such as `short`, `medium`, or `long`.

```xml
<p>{{ date | date }}</p>
<p>{{ date | date:'short' }}</p>
<p>{{ date | date:'medium' }}</p>
<p>{{ date | date:'long' }}</p>
```

### **Uppercase Pipe**

The uppercase pipe converts a string to uppercase.

```xml
<p>{{ name | uppercase }}</p>
```

### **Lowercase Pipe**

The lowercase pipe converts a string to lowercase.

```xml
<p>{{ name | lowercase }}</p>
```

## **Custom Pipes**

You can create your own custom pipes in Angular by implementing the `Pipe` interface. Here is an example of a custom pipe that capitalizes the first letter of a string:

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'capitalize'
})
export class CapitalizePipe implements PipeTransform {
  transform(value: string): string {
    if (!value) return '';
    return value.charAt(0).toUpperCase() + value.slice(1);
  }
}
```

You can use the custom pipe in your templates just like the built-in pipes:

```xml
<p>{{ name | capitalize }}</p>
```

## **Conclusion**

Angular pipes are a simple way to transform and format data in your templates. They can help you display data in a more user-friendly way, and you can even create your own custom pipes for specific needs.