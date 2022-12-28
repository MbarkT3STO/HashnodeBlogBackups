# Reactive Forms in Angular

Reactive forms, also known as model-driven forms, are a powerful way to build forms in Angular. They provide a clear separation between the form model and the template, and offer a number of advantages over template-driven forms, such as improved performance and better handling of complex forms.

In this article, we will cover the basics of reactive forms and provide some examples to help you get started.

## **Setting Up a Reactive Form**

To use reactive forms in Angular, you will need to import the `FormsModule` and `ReactiveFormsModule` from the `@angular/forms` package and add them to the `imports` array in your module.

```typescript
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    FormsModule,
    ReactiveFormsModule
  ]
})
export class AppModule { }
```

Next, you will need to create a form group and form controls. A form group is a container for a group of form controls, and a form control is an individual input field, such as a text input or a checkbox.

To create a form group, you will use the `FormGroup` class from the `@angular/forms` package and pass in a group of form controls. Here is an example of how to create a form group with two form controls:

```typescript
import { FormGroup, FormControl } from '@angular/forms';

export class MyFormComponent {
  form = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl('')
  });
}
```

## **Binding to Form Controls**

Once you have created your form group and form controls, you can bind them to your template using the `formGroup` directive. Here is an example of how to bind a form group to a template:

```xml
<form [formGroup]="form">
  <input type="text" formControlName="firstName">
  <input type="text" formControlName="lastName">
</form>
```

You can also bind individual form controls to your template using the `formControl` directive. Here is an example of how to bind a form control to a template:

```xml
<input type="text" [formControl]="form.controls.firstName">
```

## **Validating Form Controls**

Reactive forms provide a number of ways to validate form controls. You can use built-in validators, such as `required` and `minLength`, or you can create your own custom validators.

To use a built-in validator, you will need to pass it as an argument to the `FormControl` constructor. Here is an example of how to use the `required` validator:

```typescript
import { FormGroup, FormControl, Validators } from '@angular/forms';

export class MyFormComponent {
  form = new FormGroup({
    firstName: new FormControl('', Validators.required),
    lastName: new FormControl('', Validators.required)
  });
}
```

To create a custom validator, you will need to create a function that takes in a `FormControl` and returns an error object if the validation fails, or `null` if the validation passes. Here is an example of a custom validator that validates that the input is a number:

```typescript
import { FormGroup, FormControl, Validators } from '@angular/forms';

function validateNumber(control: FormControl) {
  const value = control.value;
  if (isNaN(value)) {
    return { invalidNumber: true };
  }
  return null;
}

export class MyFormComponent {
  form = new FormGroup({
    age: new FormControl('', validateNumber)
  });
}
```

You can also use multiple validators by passing an array of validators to the `FormControl` constructor. Here is an example of how to use the `required` and `minLength` validators together:

```typescript
import { FormGroup, FormControl, Validators } from '@angular/forms';

export class MyFormComponent {
  form = new FormGroup({
    password: new FormControl('', [Validators.required, Validators.minLength(8)])
  });
}
```

## **Checking the Validity of Form Controls**

To check the validity of a form control, you can use the `valid` and `invalid` properties. These properties return a boolean value indicating whether the form control is valid or invalid.

```xml
<form [formGroup]="form">
  <input type="text" formControlName="firstName" [ngClass]="{ 'is-invalid': form.controls.firstName.invalid }">
  <div *ngIf="form.controls.firstName.invalid" class="invalid-feedback">
    First name is required.
  </div>
</form>
```

You can also check the validity of the entire form using the `valid` and `invalid` properties of the `FormGroup` object.

```xml
<form [formGroup]="form">
  <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

## **Submitting the Form**

To submit the form, you can use the `ngSubmit` event binding. This will trigger a function in your component when the form is submitted.

```xml
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <button type="submit">Submit</button>
</form>
```

```typescript
export class MyFormComponent {
  form = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl('')
  });

  onSubmit() {
    console.log(this.form.value);
  }
}
```

## Real-World Example

Here is an example of a reactive form for a sign up form with validation:

```typescript
import { FormGroup, FormControl, Validators } from '@angular/forms';

export class SignUpFormComponent {
  form = new FormGroup({
    firstName: new FormControl('', Validators.required),
    lastName: new FormControl('', Validators.required),
    email: new FormControl('', [Validators.required, Validators.email]),
    password: new FormControl('', [Validators.required, Validators.minLength(8)]),
    confirmPassword: new FormControl('', [Validators.required, Validators.minLength(8)])
  }, { validators: this.passwordMatchValidator });

  passwordMatchValidator(form: FormGroup) {
    const password = form.get('password').value;
    const confirmPassword = form.get('confirmPassword').value;
    if (password !== confirmPassword) {
      return { passwordMismatch: true };
    }
    return null;
  }

  onSubmit() {
    if (this.form.valid) {
      // send form data to server
    }
  }
}
```

And here is the template for the sign up form:

```xml
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <div class="form-group">
    <label for="firstName">First Name</label>
    <input type="text" class="form-control" id="firstName" formControlName="firstName" [ngClass]="{ 'is-invalid': form.controls.firstName.invalid }">
    <div *ngIf="form.controls.firstName.invalid" class="invalid-feedback">
      First name is required.
    </div>
  </div>
  <div class="form-group">
    <label for="lastName">Last Name</label>
    <input type="text" class="form-control" id="lastName" formControlName="lastName" [ngClass]="{ 'is-invalid': form.controls.lastName.invalid }">
    <div *ngIf="form.controls.lastName.invalid" class="invalid-feedback">
      Last name is required.
    </div>
  </div>
  <div class="form-group">
    <label for="email">Email</label>
    <input type="email" class="form-control" id="email" formControlName="email" [ngClass]="{ 'is-invalid': form.controls.email.invalid }">
    <div *ngIf="form.controls.email.invalid" class="invalid-feedback">
      Email is invalid.
    </div>
  </div>
  <div class="form-group">
    <label for="password">Password</label>
    <input type="password" class="form-control" id="password" formControlName="password" [ngClass]="{ 'is-invalid': form.controls.password.invalid }">
    <div *ngIf="form.controls.password.invalid" class="invalid-feedback">
      Password is required and must be at least 8 characters long.
</div>

  </div>
  <div class="form-group">
    <label for="confirmPassword">Confirm Password</label>
    <input type="password" class="form-control" id="confirmPassword" formControlName="confirmPassword" [ngClass]="{ 'is-invalid': form.controls.confirmPassword.invalid }">
    <div *ngIf="form.controls.confirmPassword.invalid" class="invalid-feedback">
      Confirm password is required and must be at least 8 characters long.
    </div>
  </div>
  <div *ngIf="form.errors?.passwordMismatch" class="invalid-feedback">
    Passwords do not match.
  </div>
  <button type="submit" [disabled]="form.invalid" class="btn btn-primary">Sign Up</button>
</form>
```

In this example, the form has form controls for the first name, last name, email, password, and confirm password. It also has validation for each form control, as well as a custom validator that checks if the password and confirm password fields match. The template also includes error messages for each invalid form control and the form as a whole. When the form is submitted, the `onSubmit` function in the component is called, and the form data is sent to the server if the form is valid.

## **Conclusion**

Reactive forms are a powerful tool for building forms in Angular. They provide a clear separation between the form model and the template, and offer improved performance and better handling of complex forms. With the techniques covered in this article, you should have a good understanding of how to get started with reactive forms in Angular.