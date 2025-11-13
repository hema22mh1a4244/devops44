1. Write the code given below in course.ts
export class Course
{ constructor(
public courseId: number, 
public courseName:
string, public duration: 
string, public email: 
string
) { }
}
2. In the course-form.component.ts file, pass a default value to the email field as shown 
below
import { Component } from '@angular/core'; 
import { Course } from './course';
@Component({
selector: 'app-course-form',
templateUrl: './course-form.component.html', 
styleUrls: ['./course-form.component.css']
})
export class CourseFormComponent {
course: Course = new Course(1, 'Angular 2', '4 days', 'james@gmail.com');
submitted = false;
onSubmit() { this.submitted = true; }
}
3. Create a file with the name email.validator.ts under the course-form folder to 
implement custom validation functionality for the email field.
import { Directive } from '@angular/core';
import { NG_VALIDATORS, FormControl, Validator } from '@angular/forms';
@Directive({
selector: '[validateEmail]', 
providers: [
{ provide: NG_VALIDATORS, useExisting: EmailValidator, multi: true },
],
})
export class EmailValidator implements Validator { 
validate(control: FormControl): any {
const emailRegexp =
/^([a-zA-Z0-9_\-\.]+)@([a-zA-Z0-9_\-\.]+)\.([a-zA-Z]{2,5})$/;
if (!emailRegexp.test(control.value)) { 
return { emailInvalid: 'Email is invalid'
};
}
Exp. No.: Page No.:
Date: 
 Aditya College of Engineering & Technology, Surampalem (A) Roll No.
return null;
}
}
4. Add EmailValidator class in the root module i.e., app.module.ts as shown below
import { BrowserModule } from '@angular/platform-browser'; 
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { CourseFormComponent } from './course-form/course-form.component'; 
import { EmailValidator } from './course-form/email.validator';
@NgModule({
declarations: [ AppComponent, CourseFormComponent, EmailValidator],
imports: [ 
BrowserModule, 
FormsModule
],
providers: [],
bootstrap: [AppComponent]
})
export class AppModule { }
5. Add the following code in the course-form.component.html file for the email field as 
shown below
<div class="container">
<div [hidden]="submitted">
<h1>Course Form</h1>
<form (ngSubmit)="onSubmit()" #courseForm="ngForm">
<div class="form-group">
<label for="id">Course Id</label>
<input type="text" class="form-control" required [(ngModel)]="course.courseId" 
name="id" #id="ngModel">
<div [hidden]="id.valid || id.pristine" class="alert alert-danger"> 
Course Id is required</div>
</div>
<div class="form-group">
<label for="name">Course Name</label>
<input type="text" class="form-control" required 
[(ngModel)]="course.courseName" minlength="4" name="name" 
#name="ngModel">
<div *ngIf="name.errors && (name.dirty || name.touched)" class="alert alert￾danger">
<div [hidden]="!name.errors.required">Name isrequired</div>
<div [hidden]="!name.errors.minlength">Name must be at least 4 characters
long.</div>
</div>
</div>
<div class="form-group">
<label for="duration">Course Duration</label>
<input type="text" class="form-control" required [(ngModel)]="course.duration" 
Exp. No.: Page No.:
Date: 
 Aditya College of Engineering & Technology, Surampalem (A) Roll No.
name="duration" #duration="ngModel">
<div [hidden]="duration.valid || duration.pristine" class="alert
alert- danger">Duration is required</div>
</div>
<div class="form-group">
<label for="email">Author Email</label>
<input type="text" class="form-control" required
[(ngModel)]="course.email" name="email" #email="ngModel" 
validateEmail>
<div *ngIf="email.errors && (email.dirty || email.touched)" class="alert alertdanger">
<div [hidden]="!email.errors.required">Email isrequired</div>
<div [hidden]="!email.errors.emailInvalid">{{email.errors.emailInvalid}}</div>
</div>
</div>
<button type="submit" class="btn btn-primary" 
[disabled]="!courseForm.form.valid">Submit</button>
<button type="button" class="btn btn-link" 
(click)="courseForm.reset()">Reset</button>
</form>
</div>
<div [hidden]="!submitted">
<h2>You submitted the following:</h2>
<div class="row">
<div class="col-3">Course ID</div>
<div class="col-9 pull-left">{{ course.courseId }}</div>
</div>
<div class="row">
<div class="col-3">Course Name</div>
<div class="col-9 pull-left">{{ course.courseName }}</div>
</div>
<div class="row">
<div class="col-3">Duration</div>
<div class="col-9 pull-left">{{ course.duration }}</div>
</div>
<div class="row">
<div class="col-3">Email</div>
<div class="col-9 pull-left">{{ course.email }}</div>
</div>
<br>
<button class="btn btn-primary" (click)="submitted=false">Edit</button>
</div></div>
6. Save the files and check the output in the browser
