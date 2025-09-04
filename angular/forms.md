---
description: >-
  Angular Form Binding Rituals provide powerful mystical tools for handling user
  input and validation magic. Master template-driven and reactive form enchantments
  with comprehensive validation spell strategies.
---

# Angular Form Binding Rituals

## The Ancient Knowledge

Angular provides two mystical approaches to handling user input through forms: template-driven form spells and reactive form enchantments. Both capture user input events from the view, validate the user input through magical validation, create a form model and data model to update, and provide a way to track mystical changes.

## Key Mystical Concepts

- **Template-driven forms**: Use directives in the template to create and manipulate the underlying mystical object model
- **Reactive forms**: Define the form model directly in the component class and bind form controls to native form control elements
- **Form controls**: Track the value and validation status of individual form control spells
- **Form groups**: Track the same values and status for a collection of form control enchantments
- **Form arrays**: Track the same values and status for an array of form control magic
- **Validators**: Mystical functions that return null when valid or an error object when invalid

## Template-Driven Form Spells

### Basic Template-Driven Form Enchantment

```typescript
// app.module.ts
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [BrowserModule, FormsModule],
  // ...
})
export class AppModule { }

// user-form.component.ts
@Component({
  selector: 'app-user-form',
  template: `
    <div class="form-container">
      <h2>User Registration</h2>
      
      <form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)" novalidate>
        <div class="form-group">
          <label for="firstName">First Name *</label>
          <input
            type="text"
            id="firstName"
            name="firstName"
            [(ngModel)]="user.firstName"
            #firstName="ngModel"
            required
            minlength="2"
            class="form-control"
            [class.is-invalid]="firstName.invalid && firstName.touched">
          
          <div class="invalid-feedback" *ngIf="firstName.invalid && firstName.touched">
            <div *ngIf="firstName.errors?.['required']">First name is required</div>
            <div *ngIf="firstName.errors?.['minlength']">First name must be at least 2 characters</div>
          </div>
        </div>

        <div class="form-group">
          <label for="lastName">Last Name *</label>
          <input
            type="text"
            id="lastName"
            name="lastName"
            [(ngModel)]="user.lastName"
            #lastName="ngModel"
            required
            minlength="2"
            class="form-control"
            [class.is-invalid]="lastName.invalid && lastName.touched">
          
          <div class="invalid-feedback" *ngIf="lastName.invalid && lastName.touched">
            <div *ngIf="lastName.errors?.['required']">Last name is required</div>
            <div *ngIf="lastName.errors?.['minlength']">Last name must be at least 2 characters</div>
          </div>
        </div>

        <div class="form-group">
          <label for="email">Email *</label>
          <input
            type="email"
            id="email"
            name="email"
            [(ngModel)]="user.email"
            #email="ngModel"
            required
            email
            class="form-control"
            [class.is-invalid]="email.invalid && email.touched">
          
          <div class="invalid-feedback" *ngIf="email.invalid && email.touched">
            <div *ngIf="email.errors?.['required']">Email is required</div>
            <div *ngIf="email.errors?.['email']">Please enter a valid email</div>
          </div>
        </div>

        <div class="form-group">
          <label for="phone">Phone</label>
          <input
            type="tel"
            id="phone"
            name="phone"
            [(ngModel)]="user.phone"
            #phone="ngModel"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            class="form-control"
            [class.is-invalid]="phone.invalid && phone.touched"
            placeholder="123-456-7890">
          
          <div class="invalid-feedback" *ngIf="phone.invalid && phone.touched">
            <div *ngIf="phone.errors?.['pattern']">Phone must be in format: 123-456-7890</div>
          </div>
        </div>

        <div class="form-group">
          <label for="age">Age *</label>
          <input
            type="number"
            id="age"
            name="age"
            [(ngModel)]="user.age"
            #age="ngModel"
            required
            min="18"
            max="100"
            class="form-control"
            [class.is-invalid]="age.invalid && age.touched">
          
          <div class="invalid-feedback" *ngIf="age.invalid && age.touched">
            <div *ngIf="age.errors?.['required']">Age is required</div>
            <div *ngIf="age.errors?.['min']">Age must be at least 18</div>
            <div *ngIf="age.errors?.['max']">Age must be less than 100</div>
          </div>
        </div>

        <div class="form-group">
          <label for="country">Country *</label>
          <select
            id="country"
            name="country"
            [(ngModel)]="user.country"
            #country="ngModel"
            required
            class="form-control"
            [class.is-invalid]="country.invalid && country.touched">
            <option value="">Select a country</option>
            <option value="us">United States</option>
            <option value="ca">Canada</option>
            <option value="uk">United Kingdom</option>
            <option value="de">Germany</option>
            <option value="fr">France</option>
          </select>
          
          <div class="invalid-feedback" *ngIf="country.invalid && country.touched">
            <div *ngIf="country.errors?.['required']">Please select a country</div>
          </div>
        </div>

        <div class="form-group">
          <div class="form-check">
            <input
              type="checkbox"
              id="newsletter"
              name="newsletter"
              [(ngModel)]="user.newsletter"
              class="form-check-input">
            <label for="newsletter" class="form-check-label">
              Subscribe to newsletter
            </label>
          </div>
        </div>

        <div class="form-group">
          <div class="form-check">
            <input
              type="checkbox"
              id="terms"
              name="terms"
              [(ngModel)]="user.terms"
              #terms="ngModel"
              required
              class="form-check-input"
              [class.is-invalid]="terms.invalid && terms.touched">
            <label for="terms" class="form-check-label">
              I agree to the terms and conditions *
            </label>
          </div>
          
          <div class="invalid-feedback" *ngIf="terms.invalid && terms.touched">
            <div *ngIf="terms.errors?.['required']">You must agree to the terms</div>
          </div>
        </div>

        <div class="form-actions">
          <button type="submit" class="btn btn-primary" [disabled]="userForm.invalid">
            Register
          </button>
          <button type="button" class="btn btn-secondary" (click)="resetForm(userForm)">
            Reset
          </button>
        </div>
      </form>

      <div class="form-debug" *ngIf="showDebug">
        <h3>Form Debug Info</h3>
        <p>Form Valid: {{ userForm.valid }}</p>
        <p>Form Submitted: {{ userForm.submitted }}</p>
        <p>Form Values: {{ userForm.value | json }}</p>
      </div>
    </div>
  `,
  styles: [`
    .form-container {
      max-width: 600px;
      margin: 0 auto;
      padding: 20px;
    }
    .form-group {
      margin-bottom: 1rem;
    }
    .form-control {
      width: 100%;
      padding: 0.375rem 0.75rem;
      border: 1px solid #ced4da;
      border-radius: 0.25rem;
      font-size: 1rem;
      line-height: 1.5;
    }
    .form-control:focus {
      border-color: #80bdff;
      outline: 0;
      box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
    }
    .form-control.is-invalid {
      border-color: #dc3545;
    }
    .form-check {
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .form-check-input {
      margin: 0;
    }
    .invalid-feedback {
      color: #dc3545;
      font-size: 0.875rem;
      margin-top: 0.25rem;
    }
    .form-actions {
      display: flex;
      gap: 12px;
      margin-top: 2rem;
    }
    .btn {
      padding: 0.375rem 0.75rem;
      border: 1px solid transparent;
      border-radius: 0.25rem;
      cursor: pointer;
      font-size: 1rem;
    }
    .btn-primary {
      background-color: #007bff;
      border-color: #007bff;
      color: white;
    }
    .btn-primary:disabled {
      background-color: #6c757d;
      border-color: #6c757d;
    }
    .btn-secondary {
      background-color: #6c757d;
      border-color: #6c757d;
      color: white;
    }
    .form-debug {
      margin-top: 2rem;
      padding: 1rem;
      background-color: #f8f9fa;
      border-radius: 0.25rem;
    }
    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: bold;
    }
  `]
})
export class UserFormComponent {
  user = {
    firstName: '',
    lastName: '',
    email: '',
    phone: '',
    age: null,
    country: '',
    newsletter: false,
    terms: false
  };

  showDebug = false;

  onSubmit(form: NgForm): void {
    if (form.valid) {
      console.log('Form submitted:', this.user);
      // Process form data
      this.processRegistration(this.user);
    } else {
      console.log('Form is invalid');
      this.markFormGroupTouched(form);
    }
  }

  resetForm(form: NgForm): void {
    form.resetForm();
    this.user = {
      firstName: '',
      lastName: '',
      email: '',
      phone: '',
      age: null,
      country: '',
      newsletter: false,
      terms: false
    };
  }

  private markFormGroupTouched(form: NgForm): void {
    Object.keys(form.controls).forEach(key => {
      form.controls[key].markAsTouched();
    });
  }

  private processRegistration(userData: any): void {
    // Simulate API call
    console.log('Processing registration for:', userData);
  }
}

## Reactive Form Enchantments

### Basic Reactive Form Magic

```typescript
// app.module.ts
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [BrowserModule, ReactiveFormsModule],
  // ...
})
export class AppModule { }

// reactive-user-form.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, FormControl, Validators, AbstractControl } from '@angular/forms';

@Component({
  selector: 'app-reactive-user-form',
  template: `
    <div class="form-container">
      <h2>Reactive User Form</h2>

      <form [formGroup]="userForm" (ngSubmit)="onSubmit()" novalidate>
        <div class="form-group">
          <label for="firstName">First Name *</label>
          <input
            type="text"
            id="firstName"
            formControlName="firstName"
            class="form-control"
            [class.is-invalid]="isFieldInvalid('firstName')">

          <div class="invalid-feedback" *ngIf="isFieldInvalid('firstName')">
            <div *ngIf="userForm.get('firstName')?.errors?.['required']">
              First name is required
            </div>
            <div *ngIf="userForm.get('firstName')?.errors?.['minlength']">
              First name must be at least 2 characters
            </div>
          </div>
        </div>

        <div class="form-group">
          <label for="lastName">Last Name *</label>
          <input
            type="text"
            id="lastName"
            formControlName="lastName"
            class="form-control"
            [class.is-invalid]="isFieldInvalid('lastName')">

          <div class="invalid-feedback" *ngIf="isFieldInvalid('lastName')">
            <div *ngIf="userForm.get('lastName')?.errors?.['required']">
              Last name is required
            </div>
            <div *ngIf="userForm.get('lastName')?.errors?.['minlength']">
              Last name must be at least 2 characters
            </div>
          </div>
        </div>

        <div class="form-group">
          <label for="email">Email *</label>
          <input
            type="email"
            id="email"
            formControlName="email"
            class="form-control"
            [class.is-invalid]="isFieldInvalid('email')">

          <div class="invalid-feedback" *ngIf="isFieldInvalid('email')">
            <div *ngIf="userForm.get('email')?.errors?.['required']">
              Email is required
            </div>
            <div *ngIf="userForm.get('email')?.errors?.['email']">
              Please enter a valid email
            </div>
            <div *ngIf="userForm.get('email')?.errors?.['emailTaken']">
              This email is already taken
            </div>
          </div>
        </div>

        <div formGroupName="address" class="form-section">
          <h3>Address Information</h3>

          <div class="form-group">
            <label for="street">Street *</label>
            <input
              type="text"
              id="street"
              formControlName="street"
              class="form-control"
              [class.is-invalid]="isNestedFieldInvalid('address', 'street')">

            <div class="invalid-feedback" *ngIf="isNestedFieldInvalid('address', 'street')">
              <div *ngIf="userForm.get('address.street')?.errors?.['required']">
                Street is required
              </div>
            </div>
          </div>

          <div class="form-row">
            <div class="form-group col-md-6">
              <label for="city">City *</label>
              <input
                type="text"
                id="city"
                formControlName="city"
                class="form-control"
                [class.is-invalid]="isNestedFieldInvalid('address', 'city')">

              <div class="invalid-feedback" *ngIf="isNestedFieldInvalid('address', 'city')">
                <div *ngIf="userForm.get('address.city')?.errors?.['required']">
                  City is required
                </div>
              </div>
            </div>

            <div class="form-group col-md-6">
              <label for="zipCode">Zip Code *</label>
              <input
                type="text"
                id="zipCode"
                formControlName="zipCode"
                class="form-control"
                [class.is-invalid]="isNestedFieldInvalid('address', 'zipCode')">

              <div class="invalid-feedback" *ngIf="isNestedFieldInvalid('address', 'zipCode')">
                <div *ngIf="userForm.get('address.zipCode')?.errors?.['required']">
                  Zip code is required
                </div>
                <div *ngIf="userForm.get('address.zipCode')?.errors?.['pattern']">
                  Invalid zip code format
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="form-section">
          <h3>Skills</h3>
          <div formArrayName="skills">
            <div *ngFor="let skill of skillsArray.controls; let i = index"
                 [formGroupName]="i"
                 class="skill-item">
              <div class="form-row">
                <div class="form-group col-md-6">
                  <input
                    type="text"
                    formControlName="name"
                    placeholder="Skill name"
                    class="form-control">
                </div>
                <div class="form-group col-md-4">
                  <select formControlName="level" class="form-control">
                    <option value="">Select level</option>
                    <option value="beginner">Beginner</option>
                    <option value="intermediate">Intermediate</option>
                    <option value="advanced">Advanced</option>
                    <option value="expert">Expert</option>
                  </select>
                </div>
                <div class="form-group col-md-2">
                  <button type="button"
                          class="btn btn-danger btn-sm"
                          (click)="removeSkill(i)">
                    Remove
                  </button>
                </div>
              </div>
            </div>
          </div>

          <button type="button"
                  class="btn btn-secondary"
                  (click)="addSkill()">
            Add Skill
          </button>
        </div>

        <div class="form-group">
          <div class="form-check">
            <input
              type="checkbox"
              id="terms"
              formControlName="terms"
              class="form-check-input"
              [class.is-invalid]="isFieldInvalid('terms')">
            <label for="terms" class="form-check-label">
              I agree to the terms and conditions *
            </label>
          </div>

          <div class="invalid-feedback" *ngIf="isFieldInvalid('terms')">
            <div *ngIf="userForm.get('terms')?.errors?.['required']">
              You must agree to the terms
            </div>
          </div>
        </div>

        <div class="form-actions">
          <button type="submit"
                  class="btn btn-primary"
                  [disabled]="userForm.invalid || isSubmitting">
            {{ isSubmitting ? 'Submitting...' : 'Submit' }}
          </button>
          <button type="button"
                  class="btn btn-secondary"
                  (click)="resetForm()">
            Reset
          </button>
          <button type="button"
                  class="btn btn-info"
                  (click)="fillSampleData()">
            Fill Sample Data
          </button>
        </div>
      </form>

      <div class="form-debug" *ngIf="showDebug">
        <h3>Form Debug Info</h3>
        <p>Form Valid: {{ userForm.valid }}</p>
        <p>Form Status: {{ userForm.status }}</p>
        <p>Form Errors: {{ getFormErrors() | json }}</p>
        <p>Form Values: {{ userForm.value | json }}</p>
      </div>
    </div>
  `,
  styles: [`
    .form-container { max-width: 800px; margin: 0 auto; padding: 20px; }
    .form-section { margin: 2rem 0; padding: 1rem; border: 1px solid #dee2e6; border-radius: 0.25rem; }
    .form-section h3 { margin-top: 0; color: #495057; }
    .form-row { display: flex; gap: 1rem; }
    .col-md-6 { flex: 0 0 50%; }
    .col-md-4 { flex: 0 0 33.333%; }
    .col-md-2 { flex: 0 0 16.666%; }
    .skill-item { margin-bottom: 1rem; padding: 1rem; background-color: #f8f9fa; border-radius: 0.25rem; }
    .btn-sm { padding: 0.25rem 0.5rem; font-size: 0.875rem; }
    .btn-danger { background-color: #dc3545; border-color: #dc3545; color: white; }
    .btn-info { background-color: #17a2b8; border-color: #17a2b8; color: white; }
    /* Reuse styles from template-driven form */
    .form-group { margin-bottom: 1rem; }
    .form-control { width: 100%; padding: 0.375rem 0.75rem; border: 1px solid #ced4da; border-radius: 0.25rem; }
    .form-control.is-invalid { border-color: #dc3545; }
    .invalid-feedback { color: #dc3545; font-size: 0.875rem; margin-top: 0.25rem; }
    .btn { padding: 0.375rem 0.75rem; border: 1px solid transparent; border-radius: 0.25rem; cursor: pointer; }
    .btn-primary { background-color: #007bff; border-color: #007bff; color: white; }
    .btn-secondary { background-color: #6c757d; border-color: #6c757d; color: white; }
    .form-actions { display: flex; gap: 12px; margin-top: 2rem; }
    .form-debug { margin-top: 2rem; padding: 1rem; background-color: #f8f9fa; border-radius: 0.25rem; }
  `]
})
export class ReactiveUserFormComponent implements OnInit {
  userForm!: FormGroup;
  isSubmitting = false;
  showDebug = false;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.createForm();
  }

  private createForm(): void {
    this.userForm = this.fb.group({
      firstName: ['', [Validators.required, Validators.minLength(2)]],
      lastName: ['', [Validators.required, Validators.minLength(2)]],
      email: ['', [Validators.required, Validators.email], [this.emailAsyncValidator.bind(this)]],
      address: this.fb.group({
        street: ['', Validators.required],
        city: ['', Validators.required],
        zipCode: ['', [Validators.required, Validators.pattern(/^\d{5}(-\d{4})?$/)]]
      }),
      skills: this.fb.array([]),
      terms: [false, Validators.requiredTrue]
    });
  }

  get skillsArray() {
    return this.userForm.get('skills') as FormArray;
  }

  addSkill(): void {
    const skillGroup = this.fb.group({
      name: ['', Validators.required],
      level: ['', Validators.required]
    });
    this.skillsArray.push(skillGroup);
  }

  removeSkill(index: number): void {
    this.skillsArray.removeAt(index);
  }

  isFieldInvalid(fieldName: string): boolean {
    const field = this.userForm.get(fieldName);
    return !!(field && field.invalid && (field.dirty || field.touched));
  }

  isNestedFieldInvalid(groupName: string, fieldName: string): boolean {
    const field = this.userForm.get(`${groupName}.${fieldName}`);
    return !!(field && field.invalid && (field.dirty || field.touched));
  }

  onSubmit(): void {
    if (this.userForm.valid) {
      this.isSubmitting = true;
      console.log('Form submitted:', this.userForm.value);

      // Simulate API call
      setTimeout(() => {
        this.isSubmitting = false;
        console.log('Form submission completed');
      }, 2000);
    } else {
      this.markFormGroupTouched(this.userForm);
    }
  }

  resetForm(): void {
    this.userForm.reset();
    this.skillsArray.clear();
  }

  fillSampleData(): void {
    this.userForm.patchValue({
      firstName: 'John',
      lastName: 'Doe',
      email: 'john.doe@example.com',
      address: {
        street: '123 Main St',
        city: 'New York',
        zipCode: '10001'
      },
      terms: true
    });

    // Add sample skills
    this.addSkill();
    this.skillsArray.at(0).patchValue({
      name: 'Angular',
      level: 'advanced'
    });
  }

  private markFormGroupTouched(formGroup: FormGroup): void {
    Object.keys(formGroup.controls).forEach(key => {
      const control = formGroup.get(key);
      control?.markAsTouched();

      if (control instanceof FormGroup) {
        this.markFormGroupTouched(control);
      }
    });
  }

  private emailAsyncValidator(control: AbstractControl): Promise<any> {
    return new Promise((resolve) => {
      setTimeout(() => {
        if (control.value === 'taken@example.com') {
          resolve({ emailTaken: true });
        } else {
          resolve(null);
        }
      }, 1000);
    });
  }

  getFormErrors(): any {
    let errors: any = {};
    Object.keys(this.userForm.controls).forEach(key => {
      const control = this.userForm.get(key);
      if (control && !control.valid) {
        errors[key] = control.errors;
      }
    });
    return errors;
  }
}
```
```
