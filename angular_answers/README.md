# Angular Hands-On Labs Solutions

This document contains the complete source code solutions for all **10 Angular (v20.0) Hands-On Labs (HOL)**. 
All exercises in this guide build onto a single unified project: **Student Course Portal**.

---

## Table of Contents
1. [HOL 1: Environment Setup, Project Structure & First Component](#hol-1-environment-setup-project-structure--first-component)
2. [HOL 2: Data Binding, Lifecycle Hooks & Component Communication](#hol-2-data-binding-lifecycle-hooks--component-communication)
3. [HOL 3: Directives & Pipes — Built-in and Custom](#hol-3-directives--pipes--built-in-and-custom)
4. [HOL 4: Template-Driven Forms & Validation](#hol-4-template-driven-forms--validation)
5. [HOL 5: Reactive Forms — FormBuilder, FormGroup, FormArray & Custom Validators](#hol-5-reactive-forms--formbuilder-formgroup-formarray--custom-validators)
6. [HOL 6: Services & Dependency Injection](#hol-6-services--dependency-injection)
7. [HOL 7: Angular Routing — Guards, Lazy Loading & Route Data](#hol-7-angular-routing--guards-lazy-loading--route-data)
8. [HOL 8: HTTP Client — API Integration, Observables & Interceptors](#hol-8-http-client--api-integration-observables--interceptors)
9. [HOL 9: State Management — NgRx Store, Actions, Reducers, Effects & Selectors](#hol-9-state-management--ngrx-store-actions-reducers-effects--selectors)
10. [HOL 10: Unit Testing Angular Applications — Jasmine, Karma & TestBed](#hol-10-unit-testing-angular-applications--jasmine-karma--testbed)

---

## HOL 1: Environment Setup, Project Structure & First Component

### Description
Setup the base project structure and create the first core components of the Student Course Portal: `HeaderComponent`, `HomeComponent`, `CourseListComponent`, and `StudentProfileComponent`.

### File Structure
```text
student-course-portal/
├── notes.txt
└── src/
    └── app/
        ├── app.component.html
        ├── app.component.ts
        ├── app.config.ts
        ├── components/
        │   └── header/
        │       ├── header.component.html
        │       └── header.component.ts
        └── pages/
            ├── home/
            │   ├── home.component.html
            │   └── home.component.ts
            ├── course-list/
            │   ├── course-list.component.html
            │   └── course-list.component.ts
            └── student-profile/
                ├── student-profile.component.html
                └── student-profile.component.ts
```

### Source Code

#### `notes.txt`
```text
angular.json: Configures the Angular workspace defaults, build configurations, asset paths, and builder configurations.
tsconfig.json: Root TypeScript compiler options that are inherited across all applications in the workspace.
tsconfig.app.json: Specific compiler configurations tailored for compilation of the main client application.
package.json: Declares application dependencies, dev dependencies, scripts, package versions, and configuration mappings.
src/main.ts: The application entry point that bootstraps the root standalone component or AppModule.
src/app/app.config.ts: Configures global dependency injection providers for routing, animations, and HTTP services.
src/app/app.component.ts: Root component class that orchestrates structural rendering of layout outlets.
src/index.html: Principal HTML page shell that hosts the application root element and loads global stylesheets.
```

#### `src/app/components/header/header.component.ts`
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-header',
  standalone: true,
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})
export class HeaderComponent {}
```

#### `src/app/components/header/header.component.html`
```html
<nav style="background-color: #0056b3; padding: 15px; display: flex; justify-content: space-between; align-items: center; color: white;">
  <span style="font-weight: bold; font-size: 20px;">Student Course Portal</span>
  <div style="display: flex; gap: 20px;">
    <a href="#" style="color: white; text-decoration: none;">Home</a>
    <a href="#" style="color: white; text-decoration: none;">Courses</a>
    <a href="#" style="color: white; text-decoration: none;">Profile</a>
  </div>
</nav>
```

#### `src/app/pages/home/home.component.ts`
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-home',
  standalone: true,
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {}
```

#### `src/app/pages/home/home.component.html`
```html
<div style="padding: 20px; font-family: Arial, sans-serif;">
  <h1>Welcome to the Student Course Portal</h1>
  <p>Manage your courses, track your credit metrics, check grades, and maintain your academic profile.</p>
  
  <div style="display: flex; gap: 30px; margin-top: 20px;">
    <div style="border: 1px solid #ccc; padding: 15px; border-radius: 8px; text-align: center; width: 150px;">
      <h3>Courses Available</h3>
      <p style="font-size: 24px; font-weight: bold; color: #0056b3;">12</p>
    </div>
    <div style="border: 1px solid #ccc; padding: 15px; border-radius: 8px; text-align: center; width: 150px;">
      <h3>Enrolled</h3>
      <p style="font-size: 24px; font-weight: bold; color: #28a745;">3</p>
    </div>
    <div style="border: 1px solid #ccc; padding: 15px; border-radius: 8px; text-align: center; width: 150px;">
      <h3>GPA</h3>
      <p style="font-size: 24px; font-weight: bold; color: #ffc107;">3.8</p>
    </div>
  </div>
</div>
```

#### `src/app/pages/course-list/course-list.component.ts`
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-course-list',
  standalone: true,
  templateUrl: './course-list.component.html',
  styleUrls: ['./course-list.component.css']
})
export class CourseListComponent {}
```

#### `src/app/pages/course-list/course-list.component.html`
```html
<div style="padding: 20px;">
  <h2>Course Listing</h2>
  <p>Browse ongoing classes and register your interest.</p>
</div>
```

#### `src/app/pages/student-profile/student-profile.component.ts`
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-student-profile',
  standalone: true,
  templateUrl: './student-profile.component.html',
  styleUrls: ['./student-profile.component.css']
})
export class StudentProfileComponent {}
```

#### `src/app/pages/student-profile/student-profile.component.html`
```html
<div style="padding: 20px;">
  <h2>My Profile</h2>
  <p>Edit personal information and view transcript summaries.</p>
</div>
```

#### `src/app/app.component.ts`
```typescript
import { Component } from '@angular/core';
import { HeaderComponent } from './components/header/header.component';
import { HomeComponent } from './pages/home/home.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [HeaderComponent, HomeComponent],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {}
```

#### `src/app/app.component.html`
```html
<app-header></app-header>
<main>
  <app-home></app-home>
</main>
```

---

## HOL 2: Data Binding, Lifecycle Hooks & Component Communication

### Description
Implement all 4 bindings (interpolation, property, event, two-way), demonstrate basic component lifecycle logs (`ngOnInit`, `ngOnDestroy`, `ngOnChanges`), and introduce nested `@Input` and `@Output` components.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── components/
        │   └── course-card/
        │       ├── course-card.component.html
        │       ├── course-card.component.css
        │       └── course-card.component.ts
        └── pages/
            ├── home/
            │   ├── home.component.ts
            │   └── home.component.html
            └── course-list/
                ├── course-list.component.ts
                └── course-list.component.html
```

### Source Code

#### `src/app/pages/home/home.component.ts`
```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-home',
  standalone: true,
  imports: [FormsModule],
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit, OnDestroy {
  // Task 1 properties
  portalName: string = 'Student Course Portal';
  isPortalActive: boolean = true;
  message: string = '';
  searchTerm: string = '';

  // [property] (property binding) vs [(ngModel)] (two-way binding) explanation:
  // [property] represents one-way binding from component source class to the DOM attribute element.
  // [(ngModel)] represents two-way binding synchronizing updates: DOM modifications trigger model changes and vice versa.

  onEnrollClick() {
    this.message = 'Enrollment opened!';
  }

  ngOnInit() {
    // Simulate count loading and lifecycle hooks
    console.log('HomeComponent initialised — courses loaded');
  }

  ngOnDestroy() {
    console.log('HomeComponent destroyed');
  }
}
```

#### `src/app/pages/home/home.component.html`
```html
<div style="padding: 20px; font-family: Arial, sans-serif;">
  <!-- Interpolation -->
  <h1>Welcome to the {{ portalName }}</h1>
  
  <!-- Property & Event Binding -->
  <button [disabled]="!isPortalActive" (click)="onEnrollClick()">Enroll Now</button>
  <p *ngIf="message" style="color: green; font-weight: bold;">{{ message }}</p>

  <!-- Two-Way Binding -->
  <div style="margin-top: 20px;">
    <label>Search Courses: </label>
    <input [(ngModel)]="searchTerm" placeholder="Type query..." />
    <p>Searching for: <strong>{{ searchTerm }}</strong></p>
  </div>
</div>
```

#### `src/app/components/course-card/course-card.component.ts`
```typescript
import { Component, Input, Output, EventEmitter, OnChanges, SimpleChanges } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-course-card',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './course-card.component.html',
  styleUrls: ['./course-card.component.css']
})
export class CourseCardComponent implements OnChanges {
  @Input() course!: { id: number; name: string; code: string; credits: number };
  @Output() enrollRequested = new EventEmitter<number>();

  ngOnChanges(changes: SimpleChanges) {
    if (changes['course']) {
      console.log('Course changed:', {
        previous: changes['course'].previousValue,
        current: changes['course'].currentValue
      });
    }
  }
}
```

#### `src/app/components/course-card/course-card.component.html`
```html
<div style="border: 1px solid #ccc; padding: 15px; border-radius: 8px; width: 250px; background-color: #fafafa;">
  <h4>{{ course.name }}</h4>
  <p><strong>Code:</strong> {{ course.code }}</p>
  <p><strong>Credits:</strong> {{ course.credits }}</p>
  <button (click)="enrollRequested.emit(course.id)" style="cursor: pointer; background-color: #007bff; color: white; border: none; padding: 5px 10px; border-radius: 4px;">
    Enroll
  </button>
</div>
```

#### `src/app/pages/course-list/course-list.component.ts`
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { CourseCardComponent } from '../../components/course-card/course-card.component';

@Component({
  selector: 'app-course-list',
  standalone: true,
  imports: [CommonModule, CourseCardComponent],
  templateUrl: './course-list.component.html',
  styleUrls: ['./course-list.component.css']
})
export class CourseListComponent {
  courses = [
    { id: 101, name: 'Web Programming', code: 'CS101', credits: 3 },
    { id: 102, name: 'Database Foundations', code: 'CS102', credits: 4 },
    { id: 103, name: 'Systems Integration', code: 'CS103', credits: 3 },
    { id: 104, name: 'UI/UX Design Systems', code: 'CS104', credits: 2 },
    { id: 105, name: 'Cloud Native Microservices', code: 'CS105', credits: 4 }
  ];

  selectedCourseId: number | null = null;

  onEnroll(courseId: number) {
    console.log('Enrolling in course: ' + courseId);
    this.selectedCourseId = courseId;
  }
}
```

#### `src/app/pages/course-list/course-list.component.html`
```html
<div style="padding: 20px;">
  <h2>Course Listing</h2>
  <div style="display: flex; gap: 20px; flex-wrap: wrap;">
    <app-course-card 
      *ngFor="let c of courses" 
      [course]="c" 
      (enrollRequested)="onEnroll($event)"
    ></app-course-card>
  </div>

  <p *ngIf="selectedCourseId" style="margin-top: 20px; font-size: 18px; color: green;">
    Selected course ID: <strong>{{ selectedCourseId }}</strong>
  </p>
</div>
```

---

## HOL 3: Directives & Pipes — Built-in and Custom

### Description
Implement structural (`*ngIf`, `*ngFor` with `trackBy`, `*ngSwitch`) and attribute directives (`ngClass`, `ngStyle`), custom highlighting attributes, and custom string-formatting pipes.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── directives/
        │   └── highlight.directive.ts
        └── pipes/
            └── credit-label.pipe.ts
```

### Source Code

#### `src/app/directives/highlight.directive.ts`
```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  @Input() appHighlight: string = 'yellow';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string | null) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

#### `src/app/pipes/credit-label.pipe.ts`
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'creditLabel',
  standalone: true
})
export class CreditLabelPipe implements PipeTransform {
  transform(value: number | null | undefined): string {
    if (value === null || value === undefined || value === 0) {
      return 'No Credits';
    }
    return value === 1 ? '1 Credit' : `${value} Credits`;
  }
}
```

#### `src/app/components/course-card/course-card.component.ts`
```typescript
import { Component, Input, Output, EventEmitter, OnChanges, SimpleChanges } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HighlightDirective } from '../../directives/highlight.directive';
import { CreditLabelPipe } from '../../pipes/credit-label.pipe';

@Component({
  selector: 'app-course-card',
  standalone: true,
  imports: [CommonModule, HighlightDirective, CreditLabelPipe],
  templateUrl: './course-card.component.html',
  styleUrls: ['./course-card.component.css']
})
export class CourseCardComponent implements OnChanges {
  @Input() course!: { id: number; name: string; code: string; credits: number; gradeStatus: string; enrolled?: boolean };
  @Output() enrollRequested = new EventEmitter<number>();

  isExpanded: boolean = false;

  get cardClasses() {
    // Getters keep templates clean by encapsulating logic in TS rather than writing complex expressions in HTML
    return {
      'card--enrolled': this.course.enrolled,
      'card--full': this.course.credits >= 4,
      'expanded': this.isExpanded
    };
  }

  toggleExpand() {
    this.isExpanded = !this.isExpanded;
  }

  ngOnChanges(changes: SimpleChanges) {
    if (changes['course']) {
      console.log('Course changed:', changes['course'].currentValue);
    }
  }
}
```

#### `src/app/components/course-card/course-card.component.css`
```css
.card--enrolled {
  background-color: #e2f0d9 !important;
  border-color: #385723 !important;
}

.card--full {
  border-width: 3px !important;
}

.expanded {
  height: 250px;
  transition: height 0.3s ease;
}

.badge {
  padding: 5px 10px;
  border-radius: 4px;
  color: white;
  display: inline-block;
  margin-top: 5px;
  font-size: 12px;
}
.badge-passed { background-color: green; }
.badge-failed { background-color: red; }
.badge-pending { background-color: grey; }
```

#### `src/app/components/course-card/course-card.component.html`
```html
<div 
  [ngClass]="cardClasses" 
  [ngStyle]="{
    'border-left': course.gradeStatus === 'passed' ? '6px solid green' : 
                   course.gradeStatus === 'failed' ? '6px solid red' : '6px solid grey'
  }"
  [appHighlight]="'lightblue'"
  style="border: 1px solid #ccc; padding: 15px; border-radius: 8px; width: 250px; background-color: #fafafa; transition: all 0.3s ease;"
>
  <h4>{{ course.name | uppercase }}</h4>
  <p><strong>Code:</strong> {{ course.code }}</p>
  <p><strong>Credits:</strong> {{ course.credits | creditLabel }}</p>

  <!-- Switch statement for grade badge -->
  <div [ngSwitch]="course.gradeStatus">
    <span *ngSwitchCase="'passed'" class="badge badge-passed">Passed</span>
    <span *ngSwitchCase="'failed'" class="badge badge-failed">Failed</span>
    <span *ngSwitchDefault class="badge badge-pending">Pending</span>
  </div>

  <div style="margin-top: 10px; display: flex; gap: 10px;">
    <button (click)="enrollRequested.emit(course.id)" style="cursor: pointer; background-color: #007bff; color: white; border: none; padding: 5px 10px; border-radius: 4px;">
      {{ course.enrolled ? 'Unenroll' : 'Enroll' }}
    </button>
    <button (click)="toggleExpand()" style="cursor: pointer; background-color: #6c757d; color: white; border: none; padding: 5px 10px; border-radius: 4px;">
      Show Details
    </button>
  </div>
</div>
```

#### `src/app/pages/course-list/course-list.component.ts`
```typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { CourseCardComponent } from '../../components/course-card/course-card.component';

@Component({
  selector: 'app-course-list',
  standalone: true,
  imports: [CommonModule, CourseCardComponent],
  templateUrl: './course-list.component.html',
  styleUrls: ['./course-list.component.css']
})
export class CourseListComponent implements OnInit {
  courses = [
    { id: 101, name: 'Web Programming', code: 'CS101', credits: 3, gradeStatus: 'passed', enrolled: false },
    { id: 102, name: 'Database Foundations', code: 'CS102', credits: 4, gradeStatus: 'failed', enrolled: false },
    { id: 103, name: 'Systems Integration', code: 'CS103', credits: 3, gradeStatus: 'pending', enrolled: false },
    { id: 104, name: 'UI/UX Design Systems', code: 'CS104', credits: 2, gradeStatus: 'passed', enrolled: false },
    { id: 105, name: 'Cloud Native Microservices', code: 'CS105', credits: 4, gradeStatus: 'pending', enrolled: false }
  ];

  isLoading = true;
  selectedCourseId: number | null = null;

  ngOnInit() {
    setTimeout(() => {
      this.isLoading = false;
    }, 1500);
  }

  // trackBy improves list re-render performance by checking object identities
  trackByCourseId(index: number, course: any): number {
    return course.id;
  }

  onEnroll(courseId: number) {
    const course = this.courses.find(c => c.id === courseId);
    if (course) {
      course.enrolled = !course.enrolled;
    }
    this.selectedCourseId = courseId;
  }
}
```

#### `src/app/pages/course-list/course-list.component.html`
```html
<div style="padding: 20px;">
  <h2>Course Listing</h2>
  
  <div *ngIf="isLoading; else listTpl">
    <p>Loading courses...</p>
  </div>

  <ng-template #listTpl>
    <div *ngIf="courses.length === 0; else showGrid">
      <p>No courses available.</p>
    </div>
    
    <ng-template #showGrid>
      <div style="display: flex; gap: 20px; flex-wrap: wrap;">
        <app-course-card 
          *ngFor="let c of courses; trackBy: trackByCourseId" 
          [course]="c" 
          (enrollRequested)="onEnroll($event)"
        ></app-course-card>
      </div>
    </ng-template>
  </ng-template>

  <p *ngIf="selectedCourseId" style="margin-top: 20px; font-size: 18px; color: green;">
    Selected course ID: <strong>{{ selectedCourseId }}</strong>
  </p>
</div>
```

---

## HOL 4: Template-Driven Forms & Validation

### Description
Implement a template-driven form using standard `#enrollForm="ngForm"`, applying built-in validators, dirty/touched states, and custom CSS feedback.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        └── pages/
            └── enrollment-form/
                ├── enrollment-form.component.html
                ├── enrollment-form.component.css
                └── enrollment-form.component.ts
```

### Source Code

#### `src/app/pages/enrollment-form/enrollment-form.component.ts`
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule, NgForm } from '@angular/forms';

@Component({
  selector: 'app-enrollment-form',
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: './enrollment-form.component.html',
  styleUrls: ['./enrollment-form.component.css']
})
export class EnrollmentFormComponent {
  submitted = false;

  onSubmit(form: NgForm) {
    if (form.valid) {
      console.log('Form Values:', form.value);
      console.log('Form Validity:', form.valid);
      this.submitted = true;
    }
  }
}
```

#### `src/app/pages/enrollment-form/enrollment-form.component.css`
```css
.form-group {
  margin-bottom: 15px;
  width: 350px;
}

input.ng-invalid.ng-touched, select.ng-invalid.ng-touched {
  border: 1px solid red;
}

input.ng-valid.ng-touched, select.ng-valid.ng-touched {
  border: 1px solid green;
}

.error-text {
  color: red;
  font-size: 12px;
  margin-top: 5px;
}

.success-alert {
  background-color: #d4edda;
  color: #155724;
  padding: 10px;
  border-radius: 5px;
  margin-top: 15px;
  width: 350px;
}
```

#### `src/app/pages/enrollment-form/enrollment-form.component.html`
```html
<div style="padding: 20px; font-family: Arial, sans-serif;">
  <h2>Enrollment Request (Template-Driven)</h2>

  <form #enrollForm="ngForm" (ngSubmit)="onSubmit(enrollForm)">
    
    <!-- Student Name -->
    <div class="form-group">
      <label style="display: block;">Student Name:</label>
      <input 
        type="text" 
        name="studentName" 
        ngModel 
        #nameCtrl="ngModel"
        required 
        minlength="3" 
        style="width: 100%; padding: 8px;"
      />
      <div *ngIf="nameCtrl.touched && nameCtrl.invalid">
        <span *ngIf="nameCtrl.errors?.['required']" class="error-text">Name is required</span>
        <span *ngIf="nameCtrl.errors?.['minlength']" class="error-text">Name must be at least 3 characters</span>
      </div>
    </div>

    <!-- Student Email -->
    <div class="form-group">
      <label style="display: block;">Email:</label>
      <input 
        type="email" 
        name="studentEmail" 
        ngModel 
        #emailCtrl="ngModel"
        required 
        email 
        style="width: 100%; padding: 8px;"
      />
      <div *ngIf="emailCtrl.touched && emailCtrl.invalid">
        <span *ngIf="emailCtrl.errors?.['required']" class="error-text">Email is required</span>
        <span *ngIf="emailCtrl.errors?.['email']" class="error-text">Please enter a valid email</span>
      </div>
    </div>

    <!-- Course ID -->
    <div class="form-group">
      <label style="display: block;">Course ID:</label>
      <input 
        type="number" 
        name="courseId" 
        ngModel 
        #courseCtrl="ngModel"
        required 
        style="width: 100%; padding: 8px;"
      />
      <div *ngIf="courseCtrl.touched && courseCtrl.invalid">
        <span *ngIf="courseCtrl.errors?.['required']" class="error-text">Course ID is required</span>
      </div>
    </div>

    <!-- Preferred Semester -->
    <div class="form-group">
      <label style="display: block;">Preferred Semester:</label>
      <select name="preferredSemester" ngModel required style="width: 100%; padding: 8px;">
        <option value="">-- Choose Option --</option>
        <option value="Odd">Odd</option>
        <option value="Even">Even</option>
      </select>
    </div>

    <!-- Agree to Terms -->
    <div class="form-group">
      <input 
        type="checkbox" 
        name="agreeToTerms" 
        ngModel 
        #termsCtrl="ngModel"
        required 
      />
      <label style="margin-left: 5px;">I agree to term conditions.</label>
      <div *ngIf="termsCtrl.touched && termsCtrl.invalid" class="error-text">
        Acceptance is required.
      </div>
    </div>

    <div style="display: flex; gap: 10px;">
      <button type="submit" [disabled]="enrollForm.invalid">Submit</button>
      <button type="button" (click)="enrollForm.resetForm(); submitted = false;">Reset</button>
    </div>
  </form>

  <div *ngIf="submitted" class="success-alert">
    Enrollment request submitted successfully!
  </div>
</div>
```

---

## HOL 5: Reactive Forms — FormBuilder, FormGroup, FormArray & Custom Validators

### Description
Rebuild the enrollment interface using the reactive paradigm with asynchronous validation checks, custom validator predicates, and expandable input arrays.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        └── pages/
            └── reactive-enrollment-form/
                ├── reactive-enrollment-form.component.html
                ├── reactive-enrollment-form.component.css
                └── reactive-enrollment-form.component.ts
```

### Source Code

#### `src/app/pages/reactive-enrollment-form/reactive-enrollment-form.component.ts`
```typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormBuilder, FormGroup, FormArray, FormControl, Validators, AbstractControl, ValidationErrors, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-reactive-enrollment-form',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  templateUrl: './reactive-enrollment-form.component.html',
  styleUrls: ['./reactive-enrollment-form.component.css']
})
export class ReactiveEnrollmentFormComponent implements OnInit {
  enrollForm!: FormGroup;

  // enrollForm.value (excludes disabled controls) vs enrollForm.getRawValue() (includes all disabled controls as well)
  // Explanatory comment: getRawValue() yields full model mapping, preserving inputs flagged with .disable().

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    this.enrollForm = this.fb.group({
      studentName: ['', [Validators.required, Validators.minLength(3)]],
      studentEmail: ['', {
        validators: [Validators.required, Validators.email],
        asyncValidators: [this.simulateEmailCheck],
        updateOn: 'blur'
      }],
      courseId: [null, [Validators.required, this.noCourseCode]],
      preferredSemester: ['Odd', Validators.required],
      agreeToTerms: [false, Validators.requiredTrue],
      additionalCourses: this.fb.array([])
    });
  }

  // Custom synchronous validator
  noCourseCode(control: AbstractControl): ValidationErrors | null {
    const val = String(control.value || '');
    if (val.toUpperCase().startsWith('XX')) {
      return { noCourseCode: true };
    }
    return null;
  }

  // Custom asynchronous validator simulating API query
  simulateEmailCheck(control: AbstractControl): Promise<ValidationErrors | null> {
    return new Promise((resolve) => {
      setTimeout(() => {
        const email = String(control.value || '');
        if (email.includes('test@')) {
          resolve({ emailTaken: true });
        } else {
          resolve(null);
        }
      }, 800);
    });
  }

  // Typed FormArray getter
  get additionalCourses(): FormArray {
    // Typed getters eliminate the need for casting typescript objects directly inside HTML templates.
    return this.enrollForm.get('additionalCourses') as FormArray;
  }

  addCourse() {
    this.additionalCourses.push(new FormControl('', Validators.required));
  }

  removeCourse(index: number) {
    this.additionalCourses.removeAt(index);
  }

  onSubmit() {
    if (this.enrollForm.valid) {
      console.log('Value (Excludes Disabled):', this.enrollForm.value);
      console.log('Raw Value (Includes Disabled):', this.enrollForm.getRawValue());
    }
  }
}
```

#### `src/app/pages/reactive-enrollment-form/reactive-enrollment-form.component.css`
```css
.form-group {
  margin-bottom: 15px;
  width: 380px;
}

input.ng-invalid.ng-touched, select.ng-invalid.ng-touched {
  border: 1px solid red;
}

input.ng-valid.ng-touched, select.ng-valid.ng-touched {
  border: 1px solid green;
}

.error-text {
  color: red;
  font-size: 12px;
  display: block;
  margin-top: 5px;
}
```

#### `src/app/pages/reactive-enrollment-form/reactive-enrollment-form.component.html`
```html
<div style="padding: 20px; font-family: Arial, sans-serif;">
  <h2>Enrollment Request (Reactive Form)</h2>

  <form [formGroup]="enrollForm" (ngSubmit)="onSubmit()">
    
    <!-- Student Name -->
    <div class="form-group">
      <label style="display: block;">Student Name:</label>
      <input type="text" formControlName="studentName" style="width: 100%; padding: 8px;" />
      <div *ngIf="enrollForm.get('studentName')?.touched && enrollForm.get('studentName')?.invalid">
        <span *ngIf="enrollForm.get('studentName')?.errors?.['required']" class="error-text">Name is required</span>
        <span *ngIf="enrollForm.get('studentName')?.errors?.['minlength']" class="error-text">Name must be at least 3 characters</span>
      </div>
    </div>

    <!-- Student Email -->
    <div class="form-group">
      <label style="display: block;">Email:</label>
      <input type="email" formControlName="studentEmail" style="width: 100%; padding: 8px;" />
      <span *ngIf="enrollForm.get('studentEmail')?.status === 'PENDING'" style="color: blue; font-size: 11px;">Checking...</span>
      <div *ngIf="enrollForm.get('studentEmail')?.touched && enrollForm.get('studentEmail')?.invalid">
        <span *ngIf="enrollForm.get('studentEmail')?.errors?.['required']" class="error-text">Email is required</span>
        <span *ngIf="enrollForm.get('studentEmail')?.errors?.['email']" class="error-text">Valid email is required</span>
        <span *ngIf="enrollForm.get('studentEmail')?.errors?.['emailTaken']" class="error-text">Email is already taken!</span>
      </div>
    </div>

    <!-- Course ID -->
    <div class="form-group">
      <label style="display: block;">Course ID:</label>
      <input type="text" formControlName="courseId" style="width: 100%; padding: 8px;" />
      <div *ngIf="enrollForm.get('courseId')?.touched && enrollForm.get('courseId')?.invalid">
        <span *ngIf="enrollForm.get('courseId')?.errors?.['required']" class="error-text">Course ID is required</span>
        <span *ngIf="enrollForm.get('courseId')?.errors?.['noCourseCode']" class="error-text">Course code starting with XX is not allowed</span>
      </div>
    </div>

    <!-- Preferred Semester -->
    <div class="form-group">
      <label style="display: block;">Preferred Semester:</label>
      <select formControlName="preferredSemester" style="width: 100%; padding: 8px;">
        <option value="Odd">Odd</option>
        <option value="Even">Even</option>
      </select>
    </div>

    <!-- Additional Courses FormArray -->
    <div class="form-group">
      <label style="display: block; font-weight: bold; margin-bottom: 5px;">Additional Courses:</label>
      <div formArrayName="additionalCourses">
        <div *ngFor="let ctrl of additionalCourses.controls; let i=index" style="margin-bottom: 8px; display: flex; gap: 10px;">
          <input [formControlName]="i" placeholder="Course details..." style="padding: 6px; flex-grow: 1;" />
          <button type="button" (click)="removeCourse(i)">Remove</button>
        </div>
      </div>
      <button type="button" (click)="addCourse()" style="margin-top: 5px;">Add Another Course</button>
    </div>

    <!-- Agree to Terms -->
    <div class="form-group">
      <input type="checkbox" formControlName="agreeToTerms" />
      <label style="margin-left: 5px;">Agree to terms and conditions</label>
    </div>

    <button type="submit" [disabled]="enrollForm.invalid">Submit</button>
  </form>
</div>
```

---

## HOL 6: Services & Dependency Injection

### Description
Establish global state singletons using `@Injectable({ providedIn: 'root' })`, define robust data models, demonstrate service-to-service injection, and configure hierarchical providers.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── models/
        │   └── course.model.ts
        ├── services/
        │   ├── course.service.ts
        │   ├── enrollment.service.ts
        │   └── notification.service.ts
        └── components/
            ├── course-summary-widget/
            │   ├── course-summary-widget.component.ts
            │   └── course-summary-widget.component.html
            └── notification/
                ├── notification.component.ts
                └── notification.component.html
```

### Source Code

#### `src/app/models/course.model.ts`
```typescript
export interface Course {
  id: number;
  name: string;
  code: string;
  credits: number;
  gradeStatus: 'passed' | 'failed' | 'pending';
  enrolled?: boolean;
}
```

#### `src/app/services/course.service.ts`
```typescript
import { Injectable } from '@angular/core';
import { Course } from '../models/course.model';

@Injectable({
  providedIn: 'root'
})
export class CourseService {
  private courses: Course[] = [
    { id: 101, name: 'Web Programming', code: 'CS101', credits: 3, gradeStatus: 'passed' },
    { id: 102, name: 'Database Foundations', code: 'CS102', credits: 4, gradeStatus: 'failed' },
    { id: 103, name: 'Systems Integration', code: 'CS103', credits: 3, gradeStatus: 'pending' },
    { id: 104, name: 'UI/UX Design Systems', code: 'CS104', credits: 2, gradeStatus: 'passed' },
    { id: 105, name: 'Cloud Native Microservices', code: 'CS105', credits: 4, gradeStatus: 'pending' }
  ];

  getCourses(): Course[] {
    return this.courses;
  }

  getCourseById(id: number): Course | undefined {
    return this.courses.find(c => c.id === id);
  }

  addCourse(course: Course): void {
    this.courses.push(course);
  }
}
```

#### `src/app/services/enrollment.service.ts`
```typescript
import { Injectable } from '@angular/core';
import { CourseService } from './course.service';
import { Course } from '../models/course.model';

@Injectable({
  providedIn: 'root'
})
export class EnrollmentService {
  private enrolledCourseIds: number[] = [];

  // Service-to-service dependency injection
  constructor(private courseService: CourseService) {}

  enroll(courseId: number): void {
    if (!this.enrolledCourseIds.includes(courseId)) {
      this.enrolledCourseIds.push(courseId);
    }
  }

  unenroll(courseId: number): void {
    this.enrolledCourseIds = this.enrolledCourseIds.filter(id => id !== courseId);
  }

  isEnrolled(courseId: number): boolean {
    return this.enrolledCourseIds.includes(courseId);
  }

  getEnrolledCourses(): Course[] {
    const allCourses = this.courseService.getCourses();
    return allCourses.filter(c => this.enrolledCourseIds.includes(c.id));
  }
}
```

#### `src/app/services/notification.service.ts`
```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class NotificationService {
  private notifications: string[] = [];

  add(message: string) {
    this.notifications.push(message);
  }

  get() {
    return this.notifications;
  }
}
```

#### `src/app/components/course-summary-widget/course-summary-widget.component.ts`
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { CourseService } from '../../services/course.service';

@Component({
  selector: 'app-course-summary-widget',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div style="border: 2px dashed orange; padding: 15px; border-radius: 8px; width: 250px; text-align: center;">
      <h4>Course Summary Widget</h4>
      <p style="font-size: 18px;">Total System Courses: <strong>{{ coursesCount }}</strong></p>
      <button (click)="addNewTestCourse()" style="cursor: pointer;">Quick Add Test Course</button>
    </div>
  `
})
export class CourseSummaryWidgetComponent {
  constructor(private courseService: CourseService) {}

  get coursesCount() {
    return this.courseService.getCourses().length;
  }

  addNewTestCourse() {
    const id = Date.now();
    this.courseService.addCourse({
      id: id,
      name: 'Dynamic Course ' + id,
      code: 'DYN' + id.toString().substring(10),
      credits: 2,
      gradeStatus: 'pending'
    });
  }
}
```

#### `src/app/components/notification/notification.component.ts`
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { NotificationService } from '../../services/notification.service';

@Component({
  selector: 'app-notification',
  standalone: true,
  imports: [CommonModule],
  providers: [NotificationService], // Component-level provider
  template: `
    <div style="border: 1px solid gray; padding: 10px; margin-top: 15px; border-radius: 5px;">
      <h5>Component Notifications Log</h5>
      <ul>
        <li *ngFor="let note of notes">{{ note }}</li>
      </ul>
      <button (click)="logNote()">Emit Notification</button>
    </div>
  `
})
export class NotificationComponent {
  // Explanation of Component-scoped providers:
  // Declaring a service inside component 'providers' creates a new separate instance scoped exclusively to this component tree segment, bypassing root singletons.

  constructor(private notifyService: NotificationService) {}

  get notes() {
    return this.notifyService.get();
  }

  logNote() {
    this.notifyService.add(`Notice emitted at: ${new Date().toLocaleTimeString()}`);
  }
}
```

#### `src/app/components/course-card/course-card.component.ts`
```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';
import { EnrollmentService } from '../../services/enrollment.service';

@Component({
  selector: 'app-course-card',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './course-card.component.html',
  styleUrls: ['./course-card.component.css']
})
export class CourseCardComponent {
  @Input() course!: { id: number; name: string; code: string; credits: number };
  @Output() enrollRequested = new EventEmitter<number>();

  constructor(private enrollmentService: EnrollmentService) {}

  isEnrolled(): boolean {
    return this.enrollmentService.isEnrolled(this.course.id);
  }

  toggleEnroll() {
    if (this.isEnrolled()) {
      this.enrollmentService.unenroll(this.course.id);
    } else {
      this.enrollmentService.enroll(this.course.id);
    }
    this.enrollRequested.emit(this.course.id);
  }
}
```

#### `src/app/components/course-card/course-card.component.html`
```html
<div style="border: 1px solid #ccc; padding: 15px; border-radius: 8px; width: 250px; background-color: #fafafa;">
  <h4>{{ course.name }}</h4>
  <p><strong>Code:</strong> {{ course.code }}</p>
  <button (click)="toggleEnroll()" style="cursor: pointer; padding: 5px 15px; border-radius: 4px;">
    {{ isEnrolled() ? 'Unenroll' : 'Enroll' }}
  </button>
</div>
```

---

## HOL 7: Angular Routing — Guards, Lazy Loading & Route Data

### Description
Implement layout routing routing configs, URL param reading, query param bindings, auth validation checks (`CanActivate`), forms-unsaved verification checks (`CanDeactivate`), and dynamic code modules.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── app.routes.ts
        ├── pages/
        │   ├── course-detail/
        │   │   ├── course-detail.component.ts
        │   │   └── course-detail.component.html
        │   ├── not-found/
        │   │   └── not-found.component.ts
        │   └── courses-layout/
        │       └── courses-layout.component.ts
        ├── guards/
        │   ├── auth.guard.ts
        │   └── unsaved-changes.guard.ts
        └── services/
            └── auth.service.ts
```

### Source Code

#### `src/app/services/auth.service.ts`
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private loggedIn = true; // Hardcoded authentication state for demo

  isLoggedIn(): boolean {
    return this.loggedIn;
  }

  setLogin(status: boolean) {
    this.loggedIn = status;
  }
}
```

#### `src/app/guards/auth.guard.ts`
```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isLoggedIn()) {
      return true;
    }
    this.router.navigate(['/']);
    return false;
  }
}
```

#### `src/app/guards/unsaved-changes.guard.ts`
```typescript
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { Observable } from 'rxjs';

export interface CanComponentDeactivate {
  canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
}

@Injectable({
  providedIn: 'root'
})
export class UnsavedChangesGuard implements CanDeactivate<CanComponentDeactivate> {
  canDeactivate(component: CanComponentDeactivate): boolean | Observable<boolean> | Promise<boolean> {
    return component.canDeactivate ? component.canDeactivate() : true;
  }
}
```

#### `src/app/pages/not-found/not-found.component.ts`
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-not-found',
  standalone: true,
  template: `
    <div style="padding: 50px; text-align: center;">
      <h1>404 - Page Not Found</h1>
      <p>The requested URL route does not exist.</p>
    </div>
  `
})
export class NotFoundComponent {}
```

#### `src/app/pages/course-detail/course-detail.component.ts`
```typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ActivatedRoute, RouterModule } from '@angular/router';
import { CourseService } from '../../services/course.service';
import { Course } from '../../models/course.model';

@Component({
  selector: 'app-course-detail',
  standalone: true,
  imports: [CommonModule, RouterModule],
  templateUrl: './course-detail.component.html'
})
export class CourseDetailComponent implements OnInit {
  course: Course | undefined;

  constructor(private route: ActivatedRoute, private courseService: CourseService) {}

  ngOnInit() {
    // Read route parameter :id
    const idParam = this.route.snapshot.paramMap.get('id');
    if (idParam) {
      this.course = this.courseService.getCourseById(Number(idParam));
    }
  }
}
```

#### `src/app/pages/course-detail/course-detail.component.html`
```html
<div *ngIf="course; else errorTpl" style="padding: 20px; border: 1px solid #ddd; border-radius: 5px; max-width: 400px;">
  <h3>Course Detailed File</h3>
  <p><strong>Name:</strong> {{ course.name }}</p>
  <p><strong>Code:</strong> {{ course.code }}</p>
  <p><strong>Credits:</strong> {{ course.credits }}</p>
  <p><strong>Grade:</strong> {{ course.gradeStatus }}</p>
  <a routerLink="/courses" style="color: blue;">Back to Courses</a>
</div>
<ng-template #errorTpl>
  <p style="color: red;">Course detail record not found.</p>
</ng-template>
```

#### `src/app/pages/courses-layout/courses-layout.component.ts`
```typescript
import { Component } from '@angular/core';
import { RouterModule } from '@angular/router';

@Component({
  selector: 'app-courses-layout',
  standalone: true,
  imports: [RouterModule],
  template: `
    <div style="padding: 10px; background-color: #fafafa; min-height: 400px;">
      <h3>Course Management Portal</h3>
      <router-outlet></router-outlet>
    </div>
  `
})
export class CoursesLayoutComponent {}
```

#### `src/app/app.routes.ts`
```typescript
import { Routes } from '@angular/router';
import { HomeComponent } from './pages/home/home.component';
import { CoursesLayoutComponent } from './pages/courses-layout/courses-layout.component';
import { CourseListComponent } from './pages/course-list/course-list.component';
import { CourseDetailComponent } from './pages/course-detail/course-detail.component';
import { StudentProfileComponent } from './pages/student-profile/student-profile.component';
import { NotFoundComponent } from './pages/not-found/not-found.component';
import { AuthGuard } from './guards/auth.guard';
import { UnsavedChangesGuard } from './guards/unsaved-changes.guard';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { 
    path: 'courses', 
    component: CoursesLayoutComponent,
    children: [
      { path: '', component: CourseListComponent },
      { path: ':id', component: CourseDetailComponent }
    ]
  },
  { 
    path: 'profile', 
    component: StudentProfileComponent, 
    canActivate: [AuthGuard] 
  },
  { 
    path: 'enroll-reactive', 
    loadComponent: () => import('./pages/reactive-enrollment-form/reactive-enrollment-form.component').then(m => m.ReactiveEnrollmentFormComponent),
    canDeactivate: [UnsavedChangesGuard]
  },
  { path: '**', component: NotFoundComponent }
];
```

#### `src/app/pages/reactive-enrollment-form/reactive-enrollment-form.component.ts` (Modified for `CanDeactivate`)
```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators, ReactiveFormsModule } from '@angular/forms';
import { CommonModule } from '@angular/common';
import { CanComponentDeactivate } from '../../guards/unsaved-changes.guard';

@Component({
  selector: 'app-reactive-enrollment-form',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  templateUrl: './reactive-enrollment-form.component.html'
})
export class ReactiveEnrollmentFormComponent implements OnInit, CanComponentDeactivate {
  enrollForm!: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    this.enrollForm = this.fb.group({
      studentName: ['', [Validators.required, Validators.minLength(3)]],
      studentEmail: ['', [Validators.required, Validators.email]]
    });
  }

  canDeactivate(): boolean {
    if (this.enrollForm.dirty) {
      return window.confirm('You have unsaved changes. Leave?');
    }
    return true;
  }
}
```

---

## HOL 8: HTTP Client — API Integration, Observables & Interceptors

### Description
Introduce asynchronous rest calls using HttpClient providers, apply mapping pipelines, handle network errors via `catchError()`, implement retries, configure auth interceptor injection, and display global loaders.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── services/
        │   └── loading.service.ts
        ├── interceptors/
        │   ├── auth.interceptor.ts
        │   ├── error-handler.interceptor.ts
        │   └── loading.interceptor.ts
        └── components/
            └── loading-spinner/
                └── loading-spinner.component.ts
```

### Source Code

#### `src/app/services/loading.service.ts`
```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class LoadingService {
  isLoading$ = new BehaviorSubject<boolean>(false);

  show() {
    this.isLoading$.next(true);
  }

  hide() {
    this.isLoading$.next(false);
  }
}
```

#### `src/app/interceptors/auth.interceptor.ts`
```typescript
import { HttpInterceptorFn } from '@angular/common/http';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  // Clone request headers to inject authorization payload
  const authReq = req.clone({
    setHeaders: {
      Authorization: 'Bearer mock-token-12345'
    }
  });
  return next(authReq);
};
```

#### `src/app/interceptors/error-handler.interceptor.ts`
```typescript
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { Router } from '@angular/router';
import { catchError, throwError } from 'rxjs';

export const errorHandlerInterceptor: HttpInterceptorFn = (req, next) => {
  const router = inject(Router);

  return next(req).pipe(
    catchError((err) => {
      if (err.status === 401) {
        alert('Unauthorized! Redirecting home...');
        router.navigate(['/']);
      } else if (err.status === 500) {
        alert('Internal Server Error. Please contact administrator.');
      }
      return throwError(() => err);
    })
  );
};
```

#### `src/app/interceptors/loading.interceptor.ts`
```typescript
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { LoadingService } from '../services/loading.service';
import { finalize } from 'rxjs';

export const loadingInterceptor: HttpInterceptorFn = (req, next) => {
  const loadingService = inject(LoadingService);
  loadingService.show();

  return next(req).pipe(
    // Finalize runs on complete or error, acting as a try/catch/finally block
    finalize(() => loadingService.hide())
  );
};
```

#### `src/app/components/loading-spinner/loading-spinner.component.ts`
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { LoadingService } from '../../services/loading.service';

@Component({
  selector: 'app-loading-spinner',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div *ngIf="loadingService.isLoading$ | async" style="position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.3); display: flex; justify-content: center; align-items: center; z-index: 9999;">
      <div style="background: white; padding: 20px; border-radius: 8px; font-weight: bold;">Loading...</div>
    </div>
  `
})
export class LoadingSpinnerComponent {
  constructor(public loadingService: LoadingService) {}
}
```

#### `src/app/services/course.service.ts` (Modified for HttpClient)
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, map, retry, tap } from 'rxjs/operators';
import { Course } from '../models/course.model';

@Injectable({
  providedIn: 'root'
})
export class CourseService {
  private apiUrl = 'http://localhost:3000/courses';

  constructor(private http: HttpClient) {}

  getCourses(): Observable<Course[]> {
    return this.http.get<Course[]>(this.apiUrl).pipe(
      // Retry failed operations up to 2 times
      retry(2),
      // Tap for debugging side effects without mutating stream state
      tap(courses => console.log('Courses loaded:', courses.length)),
      // Map operator to filter courses
      map(courses => courses.filter(c => c.credits > 0)),
      // Standard robust error catch block
      catchError(err => {
        console.error(err);
        return throwError(() => new Error('Failed to load courses. Please try again.'));
      })
    );
  }

  getCourseById(id: number): Observable<Course> {
    return this.http.get<Course>(`${this.apiUrl}/${id}`);
  }

  createCourse(course: Omit<Course, 'id'>): Observable<Course> {
    return this.http.post<Course>(this.apiUrl, course);
  }

  updateCourse(course: Course): Observable<Course> {
    return this.http.put<Course>(`${this.apiUrl}/${course.id}`, course);
  }

  deleteCourse(id: number): Observable<any> {
    return this.http.delete(`${this.apiUrl}/${id}`);
  }
}
```

---

## HOL 9: State Management — NgRx Store, Actions, Reducers, Effects & Selectors

### Description
Implement Redux-based state management structure within the application. Create store action declarations, immutable state reducers, side-effect pipeline triggers, and selectors.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── store/
        │   ├── course/
        │   │   ├── course.actions.ts
        │   │   ├── course.reducer.ts
        │   │   ├── course.effects.ts
        │   │   └── course.selectors.ts
        │   └── enrollment/
        │       ├── enrollment.actions.ts
        │       └── enrollment.reducer.ts
```

### Source Code

#### `src/app/store/course/course.actions.ts`
```typescript
import { createAction, props } from '@ngrx/store';
import { Course } from '../../models/course.model';

export const loadCourses = createAction('[Course] Load Courses');

export const loadCoursesSuccess = createAction(
  '[Course] Load Courses Success',
  props<{ courses: Course[] }>()
);

export const loadCoursesFailure = createAction(
  '[Course] Load Courses Failure',
  props<{ error: string }>()
);
```

#### `src/app/store/course/course.reducer.ts`
```typescript
import { createReducer, on } from '@ngrx/store';
import { Course } from '../../models/course.model';
import * as CourseActions from './course.actions';

export interface CourseState {
  courses: Course[];
  loading: boolean;
  error: string | null;
}

export const initialState: CourseState = {
  courses: [],
  loading: false,
  error: null
};

export const courseReducer = createReducer(
  initialState,
  on(CourseActions.loadCourses, state => ({
    ...state,
    loading: true,
    error: null
  })),
  on(CourseActions.loadCoursesSuccess, (state, { courses }) => ({
    ...state,
    courses,
    loading: false
  })),
  on(CourseActions.loadCoursesFailure, (state, { error }) => ({
    ...state,
    error,
    loading: false
  }))
);
```

#### `src/app/store/course/course.effects.ts`
```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { CourseService } from '../../services/course.service';
import * as CourseActions from './course.actions';
import { catchError, map, mergeMap, of } from 'rxjs';

@Injectable()
export class CourseEffects {
  loadCourses$ = createEffect(() =>
    this.actions$.pipe(
      ofType(CourseActions.loadCourses),
      mergeMap(() =>
        this.courseService.getCourses().pipe(
          map(courses => CourseActions.loadCoursesSuccess({ courses })),
          catchError(err => of(CourseActions.loadCoursesFailure({ error: err.message })))
        )
      )
    )
  );

  constructor(private actions$: Actions, private courseService: CourseService) {}
}
```

#### `src/app/store/course/course.selectors.ts`
```typescript
import { createFeatureSelector, createSelector } from '@ngrx/store';
import { CourseState } from './course.reducer';

export const selectCourseState = createFeatureSelector<CourseState>('course');

export const selectAllCourses = createSelector(
  selectCourseState,
  state => state.courses
);

export const selectCoursesLoading = createSelector(
  selectCourseState,
  state => state.loading
);

export const selectCoursesError = createSelector(
  selectCourseState,
  state => state.error
);
```

#### `src/app/store/enrollment/enrollment.actions.ts`
```typescript
import { createAction, props } from '@ngrx/store';

export const enrollInCourse = createAction(
  '[Enrollment] Enroll In Course',
  props<{ courseId: number }>()
);

export const unenrollFromCourse = createAction(
  '[Enrollment] Unenroll From Course',
  props<{ courseId: number }>()
);
```

#### `src/app/store/enrollment/enrollment.reducer.ts`
```typescript
import { createReducer, on } from '@ngrx/store';
import * as EnrollmentActions from './enrollment.actions';

export interface EnrollmentState {
  enrolledCourseIds: number[];
}

export const initialState: EnrollmentState = {
  enrolledCourseIds: []
};

export const enrollmentReducer = createReducer(
  initialState,
  on(EnrollmentActions.enrollInCourse, (state, { courseId }) => ({
    ...state,
    enrolledCourseIds: [...state.enrolledCourseIds, courseId]
  })),
  on(EnrollmentActions.unenrollFromCourse, (state, { courseId }) => ({
    ...state,
    enrolledCourseIds: state.enrolledCourseIds.filter(id => id !== courseId)
  }))
);
```

---

## HOL 10: Unit Testing Angular Applications — Jasmine, Karma & TestBed

### Description
Configure testing mock providers using standard spec setups. Assert component binding outputs, emit events, inspect service implementations with `HttpTestingController`, and evaluate `MockStore` store configurations.

### File Structure Modifications
```text
student-course-portal/
└── src/
    └── app/
        ├── components/
        │   └── course-card/
        │       └── course-card.component.spec.ts
        └── services/
            └── course.service.spec.ts
```

### Source Code

#### `src/app/components/course-card/course-card.component.spec.ts`
```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';
import { CourseCardComponent } from './course-card.component';
import { SimpleChange } from '@angular/core';

describe('CourseCardComponent', () => {
  let component: CourseCardComponent;
  let fixture: ComponentFixture<CourseCardComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [CourseCardComponent]
    }).compileComponents();

    fixture = TestBed.createComponent(CourseCardComponent);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should render course name in h4', () => {
    component.course = { id: 1, name: 'Data Structures', code: 'CS101', credits: 4, gradeStatus: 'passed' };
    fixture.detectChanges();

    const titleEl = fixture.debugElement.query(By.css('h4')).nativeElement;
    expect(titleEl.textContent).toContain('DATA STRUCTURES');
  });

  it('should emit enrollRequested event when button is clicked', () => {
    component.course = { id: 1, name: 'Data Structures', code: 'CS101', credits: 4, gradeStatus: 'passed' };
    fixture.detectChanges();

    spyOn(component.enrollRequested, 'emit');
    const button = fixture.debugElement.query(By.css('button')).nativeElement;
    button.click();

    expect(component.enrollRequested.emit).toHaveBeenCalledWith(1);
  });

  it('should log previous and current values on ngOnChanges', () => {
    spyOn(console, 'log');
    
    const prevCourse = { id: 1, name: 'Data Structures', code: 'CS101', credits: 4, gradeStatus: 'passed' };
    const currCourse = { id: 1, name: 'Algorithms', code: 'CS102', credits: 4, gradeStatus: 'passed' };
    
    component.course = currCourse;
    component.ngOnChanges({
      course: new SimpleChange(prevCourse, currCourse, false)
    });

    expect(console.log).toHaveBeenCalled();
  });
});
```

#### `src/app/services/course.service.spec.ts`
```typescript
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { CourseService } from './course.service';
import { Course } from '../models/course.model';

describe('CourseService', () => {
  let service: CourseService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [CourseService]
    });

    service = TestBed.inject(CourseService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    // Assert no outstanding unresolved requests exist
    httpMock.verify();
  });

  it('should retrieve courses via GET', () => {
    const mockCourses: Course[] = [
      { id: 101, name: 'Web Programming', code: 'CS101', credits: 3, gradeStatus: 'passed' },
      { id: 102, name: 'Database Foundations', code: 'CS102', credits: 4, gradeStatus: 'failed' }
    ];

    service.getCourses().subscribe(courses => {
      expect(courses.length).toBe(2);
      expect(courses).toEqual(mockCourses);
    });

    const req = httpMock.expectOne('http://localhost:3000/courses');
    expect(req.request.method).toBe('GET');
    req.flush(mockCourses);
  });

  it('should handle network errors gracefully', () => {
    service.getCourses().subscribe({
      next: () => fail('should have failed with 500 error'),
      error: (err) => {
        expect(err.message).toContain('Failed to load courses');
      }
    });

    const req = httpMock.expectOne('http://localhost:3000/courses');
    req.flush('Database connection failed', { status: 500, statusText: 'Internal Server Error' });
  });
});
```
