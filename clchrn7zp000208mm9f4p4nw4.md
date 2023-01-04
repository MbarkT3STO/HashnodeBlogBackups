# Angular ngSwitch Directive

The Angular `ngSwitch` directive is used to conditionally apply styles to an element based on the value of an expression. It works by evaluating an expression and then rendering a DOM element based on the resulting value.

Here is an example of using `ngSwitch` to display a different message based on the value of a variable:

```xml
<div [ngSwitch]="variable">
  <p *ngSwitchCase="'value1'">This is case 1</p>
  <p *ngSwitchCase="'value2'">This is case 2</p>
  <p *ngSwitchDefault>This is the default case</p>
</div>
```

In this example, if `variable` is equal to `'value1'`, the `div` element will display the message "This is case 1". If `variable` is equal to `'value2'`, the message "This is case 2" will be displayed. If `variable` has any other value, the default message "This is the default case" will be displayed.

## **Syntax**

The syntax for using `ngSwitch` is as follows:

```xml
<element [ngSwitch]="expression">
  <template ngSwitchCase="value1">...</template>
  <template ngSwitchCase="value2">...</template>
  <template ngSwitchDefault>...</template>
</element>
```

The `ngSwitch` directive is applied to an element and takes an expression as its value. The expression is then evaluated, and based on the result, one of the `ngSwitchCase` templates or the `ngSwitchDefault` template is rendered.

## **Using ngSwitchCase**

The `ngSwitchCase` directive is used to specify a template to be rendered if the expression evaluated by `ngSwitch` matches the value specified in the `ngSwitchCase` directive.

For example:

```xml
<div [ngSwitch]="variable">
  <p *ngSwitchCase="'value1'">This is case 1</p>
  <p *ngSwitchCase="'value2'">This is case 2</p>
</div>
```

In this example, if `variable` is equal to `'value1'`, the `div` element will display the message "This is case 1". If `variable` is equal to `'value2'`, the message "This is case 2" will be displayed.

## **Using ngSwitchDefault**

The `ngSwitchDefault` directive is used to specify a template to be rendered if the expression evaluated by `ngSwitch` does not match any of the values specified in the `ngSwitchCase` directives.

For example:

```xml
<div [ngSwitch]="variable">
  <p *ngSwitchCase="'value1'">This is case 1</p>
  <p *ngSwitchCase="'value2'">This is case 2</p>
  <p *ngSwitchDefault>This is the default case</p>
</div>
```

In this example, if `variable` is equal to `'value1'`, the `div` element will display the message "This is case 1". If `variable` is equal to `'value2'`, the message "This is case 2" will be displayed. If `variable` has any other value, the default message "This is the default case" will be displayed.