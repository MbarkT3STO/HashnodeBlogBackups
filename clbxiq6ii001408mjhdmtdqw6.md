# What is Angular Property Binding?

## **Introduction**

Angular property binding is a way to bind a component's property to an element's property in the template. It allows you to set the value of an element's property in the template based on the component's data.

Property binding is a one-way data binding technique, meaning that data flows from the component to the template, but not the other way around. It is often used to bind the component's data to the template's elements.

## **Using Property Binding**

To use property binding, you can use the `[property]` syntax in the template and bind it to a component's property. For example:

```xml
<button [disabled]="isDisabled">Click me</button>
```

In this example, the `isDisabled` property of the component is bound to the `disabled` property of the `<button>` element. If the value of `isDisabled` is `true`, the `<button>` element will be disabled. If the value of `isDisabled` is `false`, the `<button>` element will be enabled.

You can also use property binding with expressions, such as:

```xml
<img [src]="imageUrl + '?v=' + version">
```

In this example, the `imageUrl` and `version` properties of the component are used in an expression to set the `src` property of the `<img>` element. The expression concatenates the values of `imageUrl` and `version` to create the final URL for the image.

## **Property Binding and Security**

Property binding is safe to use, as it does not allow the execution of arbitrary code. It only allows the setting of element properties based on data values and expressions.

# **Conclusion**

Angular property binding is a powerful way to bind a component's data to the template's elements. It allows you to set the value of an element's property in the template based on the component's data using the `[property]` syntax and expressions. Property binding is a one-way data binding technique that is safe to use, as it does not allow the execution of arbitrary code.