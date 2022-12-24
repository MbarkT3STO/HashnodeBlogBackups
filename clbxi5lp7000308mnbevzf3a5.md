# Data Binding in Angular

## **Introduction**

Data binding is a fundamental concept in Angular that allows the flow of data between a component and its template. It helps to bind the component's data and properties to the template, making it easy to display and interact with the component's data.

There are two types of data binding in Angular:

*   **One-way data binding**: This type of data binding flows data in one direction, from the component to the template. One-way data binding is used to display data in the template and is often used with the `{{ }}` interpolation syntax.
    
*   **Two-way data binding**: This type of data binding flows data in both directions, between the component and the template. Two-way data binding is used to bind form inputs to component properties and is often used with the `[(ngModel)]` directive.
    

## **One-Way Data Binding**

One-way data binding is used to display data in the template. It allows the component to pass data to the template, but the template cannot update the component's data.

To use one-way data binding, you can use the `{{ }}` interpolation syntax in the template to display a component's property. For example:

```xml
<p>{{ componentProperty }}</p>
```

This will display the value of `componentProperty` in the template.

## **Two-Way Data Binding**

Two-way data binding is used to bind form inputs to component properties. It allows the template to update the component's data and the component to update the template's data.

To use two-way data binding, you can use the `[(ngModel)]` directive in the template to bind a form input to a component property. For example:

```xml
<input type="text" [(ngModel)]="componentProperty">
```

This will bind the value of the form input to the `componentProperty` in the component, allowing the template to update the component's data and the component to update the template's data.

# **Conclusion**

Data binding is an important concept in Angular that allows the flow of data between a component and its template. It helps to bind the component's data and properties to the template, making it easy to display and interact with the component's data. By using one-way and two-way data binding, you can easily display and update data in your Angular templates.