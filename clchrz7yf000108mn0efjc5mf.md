# Angular ngClass Directive

The Angular ngClass directive is a powerful way to dynamically set the CSS class of an element based on expressions and conditions in your component. This can be especially useful for styling elements based on the state of your application or for applying styles based on user interactions.

## **Basic Syntax**

The basic syntax for using the ngClass directive is as follows:

```xml
<element [ngClass]="expression">Content</element>
```

The expression inside the ngClass directive should evaluate to an object, an array, or a string.

## **Using an Object**

One way to use the ngClass directive is to bind it to an object that maps CSS class names to boolean values. If the value is `true`, the corresponding class will be applied to the element. If the value is `false`, the class will not be applied.

Here is an example of using an object with the ngClass directive:

```xml
<div [ngClass]="{'highlight': isHighlighted}">Text to be highlighted</div>
```

In this example, the `highlight` class will be applied to the `div` element if the `isHighlighted` variable is `true`, and it will not be applied if `isHighlighted` is `false`.

## **Using an Array**

Another way to use the ngClass directive is to bind it to an array of class names. If an element of the array is a string, it will be treated as a class name. If it is an object, it should contain a key-value pair where the key is the class name and the value is a boolean.

Here is an example of using an array with the ngClass directive:

```xml
<div [ngClass]="[{'highlight': isHighlighted}, 'bold']">Text to be highlighted and bold</div>
```

In this example, the `highlight` class will be applied to the `div` element if the `isHighlighted` variable is `true`, and the `bold` class will always be applied.

## **Using a String**

The ngClass directive can also be bound to a string containing a list of class names separated by spaces.

Here is an example of using a string with the ngClass directive:

```xml
<div [ngClass]="'highlight bold'">Text to be highlighted and bold</div>
```

In this example, both the `highlight` and `bold` classes will be applied to the `div` element.

## **Conclusion**

The Angular ngClass directive is a useful tool for dynamically applying CSS classes to elements based on expressions and conditions. Whether you use an object, an array, or a string, the ngClass directive makes it easy to style your application based on its state and user interactions.