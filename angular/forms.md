# Forms and validation

## Template-driven forms and reactive forms 

Template-driven forms and reactive forms are two approaches to form handling in Angular. Here's a comparison of the two:

Template-driven Forms:
- Template-driven forms rely heavily on the template (HTML) to handle form validation and data binding.
- Form controls are automatically created and managed by Angular based on directives such as `ngModel`.
- Form validation is declarative and defined in the template using directives like `required`, `min`, `max`, etc.
- The form logic is handled within the template itself, making it simpler and quicker to implement for small to medium-sized forms.
- Template-driven forms are suitable for scenarios where the form logic is straightforward and tightly coupled to the UI.

Reactive Forms:
- Reactive forms focus on handling form validation and data binding programmatically within the component class.
- Form controls and their state are explicitly defined and managed by the developer in the component class.
- Form validation is implemented using validators provided by Angular, or custom validators can be created.
- The form logic is handled in a separate component class, providing more flexibility and control over form handling.
- Reactive forms are suitable for complex forms with dynamic validation requirements and scenarios where form logic needs to be reused across multiple components.

Key Differences:
- Control: Template-driven forms are more template-centric, while reactive forms are more code-centric.
- Control Over Validation: Reactive forms provide more control and flexibility over form validation by allowing custom validators and dynamic validation logic.
- Testing: Reactive forms are easier to test as the form logic is decoupled from the template.
- Data Model: Reactive forms provide a more explicit and centralized data model that reflects the form structure.
- Flexibility: Reactive forms provide more flexibility for implementing advanced features like conditional validation, dynamic form controls, and form arrays.

The choice between template-driven forms and reactive forms depends on the complexity and requirements of your form. Template-driven forms offer simplicity and ease of implementation for simple forms, while reactive forms provide more control and flexibility for complex forms with dynamic requirements.

## Reactive forms

### Built-in validators

In Angular's reactive forms, built-in validators are pre-defined functions that allow you to perform common form validations without having to write custom validation logic. These validators are provided by the `Validators` class from `@angular/forms` module. They can be used to validate form controls and apply validation rules to fields such as required, min length, max length, pattern matching, and more. Here are some examples of built-in validators:

1. `required`: Ensures that the form control has a non-empty value.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  name: ['', Validators.required]
});
```

2. `minLength`: Specifies the minimum length of the input value.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  password: ['', [Validators.minLength(6)]]
});
```

3. `maxLength`: Specifies the maximum length of the input value.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  name: ['', [Validators.maxLength(20)]]
});
```

4. `pattern`: Validates the input value against a regular expression pattern.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  email: ['', [Validators.pattern('^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}$')]]
});
```

5. `email`: Validates that the input value is a valid email address.
```typescript
import { Validators } from '@angular/forms';

// Example usage
this.myForm = this.formBuilder.group({
  email: ['', [Validators.email]]
});
```

You can combine multiple validators by passing an array of validator functions to the form control or form group.

```typescript
import { Validators } from '@angular/forms';

// Example usage with multiple validators
this.myForm = this.formBuilder.group({
  password: ['', [Validators.required, Validators.minLength(6)]],
  confirmPassword: ['', Validators.required]
});
```

Using these built-in validators simplifies the process of adding basic form validation rules to your reactive forms. However, if you have custom validation requirements, you can create your own custom validators by implementing the `ValidatorFn` interface. Custom validators give you full control over the validation logic for your specific use cases.

Here is a complete list of built-in validators available in Angular's reactive forms:

- `required`: Validates that the control has a non-empty value.
- `requiredTrue`: Validates that the control has a value of `true`. Typically used for checkbox inputs.
- `minLength`: Validates that the control value has a minimum length specified.
- `maxLength`: Validates that the control value has a maximum length specified.
- `pattern`: Validates that the control value matches a specific regular expression pattern.
- `email`: Validates that the control value is a valid email address.
- `min`: Validates that the control value is greater than or equal to a specified minimum value.
- `max`: Validates that the control value is less than or equal to a specified maximum value.
- `nullValidator`: Validates that the control value is `null`.
- `compose`: Combines multiple validators into a single validator function.

These validators are part of the `Validators` class provided by the `@angular/forms` module. They can be used individually or in combination to enforce specific validation rules on form controls.

By using these built-in validators, you can easily add common validation rules to your reactive forms without writing custom validation logic.


### Custom validators

In Angular's reactive forms, you can create custom validators to implement specific validation logic beyond the built-in validators. Custom validators allow you to define your own validation rules and apply them to form controls. Here's how you can create a custom validator:

1. Create a function that implements the `ValidatorFn` interface. This function will be responsible for performing the validation logic.

```typescript
import { AbstractControl, ValidatorFn } from '@angular/forms';

export function customValidator(): ValidatorFn {
  return (control: AbstractControl): { [key: string]: any } | null => {
    // Validation logic goes here
    const isValid = /* perform validation based on control value */;
    
    // Return validation error if invalid
    return isValid ? null : { customValidation: true };
  };
}
```

2. Use the custom validator function in your form controls by passing it as an argument to the `Validators` array.

```typescript
import { Validators } from '@angular/forms';
import { customValidator } from './custom-validator';

this.myForm = this.formBuilder.group({
  name: ['', [Validators.required, customValidator()]]
});
```

In the example above, the `customValidator` function is used to create a custom validator that checks whether the control value meets a specific validation criteria. If the validation fails, the function returns an error object with a key (`customValidation`) to indicate the validation failure.

You can also pass parameters to your custom validator function if needed. For example, if your validation logic requires a specific value or additional data, you can modify the `customValidator` function to accept parameters.

```typescript
export function customValidator(param: any): ValidatorFn {
  return (control: AbstractControl): { [key: string]: any } | null => {
    // Validation logic using the parameter
    const isValid = /* perform validation based on control value and param */;
    
    // Return validation error if invalid
    return isValid ? null : { customValidation: true };
  };
}
```

Then, when using the custom validator, you can pass the required parameter.

```typescript
this.myForm = this.formBuilder.group({
  name: ['', [Validators.required, customValidator('parameter')]]
});
```

Custom validators provide you with the flexibility to define and apply your own validation rules in reactive forms, allowing you to meet specific validation requirements for your application.

### Form condition checks

In Angular's reactive forms, you can perform condition checks on form controls and form groups to determine their state and validity. These condition checks allow you to display or hide specific elements or apply certain styles based on the form's state. Here are some common condition checks you can use:

1. `formControlName.invalid`: Checks if a specific form control is invalid. For example:

```html
<div *ngIf="myForm.get('name').invalid">
  <!-- Display error message or apply styles -->
</div>
```

2. `formControlName.valid`: Checks if a specific form control is valid.

```html
<div *ngIf="myForm.get('name').valid">
  <!-- Display success message or apply styles -->
</div>
```

3. `formControlName.dirty`: Checks if a specific form control has been modified (dirty).

```html
<div *ngIf="myForm.get('name').dirty">
  <!-- Apply styles or show an indicator -->
</div>
```

4. `formControlName.pristine`: Checks if a specific form control has not been modified (pristine).

```html
<div *ngIf="myForm.get('name').pristine">
  <!-- Apply styles or show an indicator -->
</div>
```

5. `formControlName.touched`: Checks if a specific form control has been touched (interacted with by the user).

```html
<div *ngIf="myForm.get('name').touched">
  <!-- Apply styles or show an indicator -->
</div>
```

6. `formControlName.untouched`: Checks if a specific form control has not been touched.

```html
<div *ngIf="myForm.get('name').untouched">
  <!-- Apply styles or show an indicator -->
</div>
```

These condition checks can be used within Angular's template syntax, such as `*ngIf` directives, to conditionally display elements or apply specific styles based on the form's state. By leveraging these condition checks, you can create dynamic and interactive forms that respond to user input and provide visual feedback based on the form's validity and state.


### Displaying validation errors dynamically


To dynamically display validation errors, you can leverage Angular's form control state and the `errors` property. Here's an example of how you can achieve this:

1. Define your form controls and apply the desired validators. For example, let's assume you have a form control for the name field:

```typescript
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

// Inside your component class
myForm: FormGroup;

constructor(private formBuilder: FormBuilder) {
  this.myForm = this.formBuilder.group({
    name: ['', Validators.required]
  });
}
```

2. In your component's template, bind the form control to an input field and display validation errors dynamically. You can use the `errors` property of the form control to access the validation errors.

```html
<form [formGroup]="myForm">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" formControlName="name">

    <div *ngIf="myForm.get('name').invalid && (myForm.get('name').dirty || myForm.get('name').touched)">
      <div *ngIf="myForm.get('name').errors.required">Name is required.</div>
    </div>
  </div>
</form>
```

In the above code, the `*ngIf` directives are used to conditionally display the error messages. The `myForm.get('name').invalid` condition checks if the form control is invalid, while `(myForm.get('name').dirty || myForm.get('name').touched)` ensures that the error is displayed only when the user has interacted with the field.

The nested `*ngIf` checks for the specific validation error. In this case, it checks for the `required` error and displays the corresponding error message.

As the user inputs data and triggers the form control's validation, Angular automatically updates the `errors` property, and the template reacts to those changes by displaying or hiding the validation error message dynamically.

By following this approach, you can display validation errors in real-time as the user interacts with the form, providing immediate feedback on the input validity.

### Form submission

In Angular's reactive forms, handling form submission involves listening to the form's `submit` event and performing the desired actions, such as submitting data to a server or performing additional logic. Here's how you can handle form submission with reactive forms:

1. Add a submit event handler to your form element in the template:

```html
<form [formGroup]="myForm" (ngSubmit)="onSubmit()">
  <!-- Form controls and markup -->
  <button type="submit">Submit</button>
</form>
```

2. In your component class, define the `onSubmit` method and handle the form submission logic:

```typescript
import { FormGroup } from '@angular/forms';

// Inside your component class
myForm: FormGroup;

onSubmit() {
  if (this.myForm.valid) {
    // Perform actions when the form is valid
    const formData = this.myForm.value;
    // Submit data to a server, perform additional logic, etc.
  } else {
    // Handle form errors or display validation messages
  }
}
```

In the `onSubmit` method, you can check if the form is valid using the `valid` property of the form group. If the form is valid, you can access the form data using the `value` property of the form group. You can then perform the necessary actions, such as submitting the data to a server or performing additional logic.

If the form is invalid, you can handle form errors or display validation messages to the user. You can access the specific form control errors using the `errors` property of each form control.

By default, when the user clicks the submit button or presses Enter within a form field, the `ngSubmit` event will be triggered, invoking the `onSubmit` method. Ensure that your form controls have the appropriate validators to enforce the desired validation rules before submitting the form.

Remember to import the necessary modules and dependencies for reactive forms in your component file.

With this approach, you can handle form submission with reactive forms in Angular and perform the necessary actions based on the form's validity and the user's input.

#### Example

Here's an example of a login form with email and password inputs, along with validations, using reactive forms in Angular:

**login.component.html:**
```html
<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" formControlName="email" [ngClass]="{ 'is-invalid': isInvalid('email') }">
    <div *ngIf="isInvalid('email')" class="invalid-feedback">
      <div *ngIf="loginForm.get('email').errors.required">Email is required.</div>
      <div *ngIf="loginForm.get('email').errors.email">Please enter a valid email.</div>
    </div>
  </div>
  <div>
    <label for="password">Password:</label>
    <input type="password" id="password" formControlName="password" [ngClass]="{ 'is-invalid': isInvalid('password') }">
    <div *ngIf="isInvalid('password')" class="invalid-feedback">
      <div *ngIf="loginForm.get('password').errors.required">Password is required.</div>
      <div *ngIf="loginForm.get('password').errors.minlength">Password should have at least 8 characters.</div>
      <div *ngIf="loginForm.get('password').errors.maxlength">Password should have at most 20 characters.</div>
    </div>
  </div>
  <div>
    <button type="submit" [disabled]="loginForm.invalid">Log In</button>
  </div>
</form>
```

**login.component.css:**
```css
.is-invalid {
  border: 1px solid red;
}

.invalid-feedback {
  color: red;
}
```

**login.component.ts:**
```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent implements OnInit {
  loginForm: FormGroup;

  constructor(private formBuilder: FormBuilder) {}

  ngOnInit() {
    this.loginForm = this.formBuilder.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', [Validators.required, Validators.minLength(8), Validators.maxLength(20)]]
    });
  }

  onSubmit() {
    if (this.loginForm.valid) {
      // Perform login logic
      const email = this.loginForm.value.email;
      const password = this.loginForm.value.password;
      // Send login request to server or perform other actions
    } else {
      // Handle form errors or display validation messages
    }
  }

  isInvalid(controlName: string) {
    const control = this.loginForm.get(controlName);
    return control.invalid && (control.dirty || control.touched);
  }
}
```

In this example, the `loginForm` FormGroup is created with two form controls: `email` and `password`. Each form control has associated validators to enforce the required rules, such as `Validators.required`, `Validators.email`, `Validators.minLength`, and `Validators.maxLength`. The form controls are bound to the input fields using the `formControlName` directive.

The `[ngClass]` directive is used to apply the `is-invalid` CSS class to the input fields when they are invalid. The error messages are displayed dynamically based on the form control errors using `*ngIf` directives.

The submit button is disabled when the form is invalid, indicated by `[disabled]="loginForm.invalid"`.

In the component class,

 the `onSubmit` method is invoked when the form is submitted. If the form is valid, the login logic can be performed. Otherwise, you can handle form errors or display validation messages.

The `isInvalid` method is used to check if a specific form control is invalid and has been touched or modified by the user. It returns `true` if the control is invalid and either dirty (modified) or touched (blurred).

Remember to import the necessary modules and dependencies for reactive forms in your component file.

With this example, you have a complete login form with email and password inputs, along with validations and dynamic error messages. The submit button is enabled only when the form inputs are valid.

#### Form data manipulation before sending

In Angular's reactive forms, you can perform data manipulation before sending the form data to the server. This can be done in the `onSubmit` method or any other appropriate method in your component. Here's how you can perform form data manipulation with reactive forms:

1. Set up your form controls and validators in your component:

```typescript
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

// Inside your component class
myForm: FormGroup;

constructor(private formBuilder: FormBuilder) {}

ngOnInit() {
  this.myForm = this.formBuilder.group({
    // Define your form controls and validators
    firstName: ['', Validators.required],
    lastName: ['', Validators.required],
    // Other form controls
  });
}
```

2. Define the `onSubmit` method and perform data manipulation before sending the form data:

```typescript
onSubmit() {
  if (this.myForm.valid) {
    // Perform data manipulation
    const formData = this.myForm.value;
    formData.fullName = formData.firstName + ' ' + formData.lastName;
    // Other data manipulation as needed

    // Send the modified form data to the server or perform other actions
    this.myService.submitForm(formData).subscribe(
      response => {
        // Handle the server response
      },
      error => {
        // Handle the error
      }
    );
  } else {
    // Handle form errors
  }
}
```

In the `onSubmit` method, you can access the form data using the `value` property of the form group (`this.myForm.value`). This gives you an object containing the form control values. You can then perform any desired data manipulation before sending it to the server.

In the example above, we concatenate the `firstName` and `lastName` form control values to create a `fullName` property in the form data object. This manipulated data can be used for displaying or sending to the server.

After performing the necessary data manipulation, you can then send the modified form data to the server using an HTTP request, or perform any other actions as needed.

Remember to handle form validation and errors before performing the data manipulation to ensure the data is valid and meets the required criteria.

By manipulating the form data before sending it to the server, you can customize and format the data according to your application's requirements.



