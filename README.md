# angular

import { Component, OnInit } from '@angular/core';
import { FormGroup, FormControl, FormBuilder, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit {
  master_checked = {
    groupOne: false,
    groupTwo: false,
    groupThree: false
  };
  master_indeterminate = {
    groupOne: false,
    groupTwo: false,
    groupThree: false
  };
  checkbox_list = [];
  checkboxForm: FormGroup;

  constructor(private fb: FormBuilder) {
    this.checkbox_list = [
      {
        name: "HTML/CSS",  
        formName: "html",
        disabled: false,
        checked: false,
        labelPosition: "after"
      }, {
        name: "JavaScript/jQuery",
        formName: "javascript",
        disabled: false,
        checked: false,
        labelPosition: "after"
      }, {
        name: "CSS Preprocessing",
        formName: "css",
        disabled: false,
        checked: false,
        labelPosition: "after"
      },{
        name: "Version Control/Git",
        formName:"git",
        disabled: false,
        checked: false,
        labelPosition: "after"
      }, 
    ]
  }

  ngOnInit() {
    this.checkboxForm = this.fb.group({
      groupOne: this.fb.group({
        master: false,
        html: false,
        javascript: false,
        css: false,
        git: false
      }),
      groupTwo:  this.fb.group({
        master: false,
        html: false,
        javascript: false,
        css: false,
        git: false
      }),
      groupThree:  this.fb.group({
        master: false,
        html: false,
        javascript: false,
        css: false,
        git: false
      })
    });
  }

  master_change(groupName: string) {
    this.master_checked[groupName] = !this.master_checked[groupName];
    const group = this.checkboxForm.get(groupName) as FormGroup;
    for (let index of Object.keys(group.controls)) {
      if (index === "master") {
        continue;
      }
      group.controls[index].setValue(this.master_checked[groupName]);
    }
  }

  list_change(groupName: string) { 
    let checked_count = 0;
    const group = this.checkboxForm.get(groupName) as FormGroup;
    const values = Object.keys(group.controls)
    .filter((fcn) => fcn !== "master")
    .map((fcn) => group.controls[fcn].value);

    checked_count += values.filter(Boolean).length;

    if (checked_count > 0 && checked_count < this.checkbox_list.length) {
      // If some checkboxes are checked but not all; then set Indeterminate state of the master to true.
      this.master_indeterminate[groupName] = true;
    } else if (checked_count == this.checkbox_list.length) {
      //If checked count is equal to total items; then check the master checkbox and also set Indeterminate state to false.
      this.master_indeterminate[groupName] = false;
      this.master_checked[groupName] = true;
      group.controls["master"].setValue(true);
    } else {
      //If none of the checkboxes in the list is checked then uncheck master also set Indeterminate to false.
      this.master_indeterminate[groupName] = false;
      this.master_checked[groupName] = false;
      group.controls["master"].setValue(false);
    }
  }
}




<form [formGroup]="checkboxForm">
<div style="padding: 50px; width:400px;">

    <pre>Value: {{checkboxForm.value | json}}</pre>

    <mat-card>
      <mat-card-title>Angular Material 8 Checkboxes</mat-card-title>
      <mat-card-content class="form-group">
        <div>
          <mat-list>
            <mat-list-item>
              <span>Functions</span>
            </mat-list-item>

            <mat-list-item *ngFor="let item of checkbox_list" class="alternate-row">
              <span>{{ item.name }}</span>
            </mat-list-item>
          </mat-list>
        </div>
        <div formGroupName="groupOne">
            <mat-list>
              <mat-list-item>
                <mat-checkbox
                color="primary"
                [(indeterminate)]="master_indeterminate.groupOne"
                (change)="master_change('groupOne')"
                formControlName="master"
                ><b>1</b></mat-checkbox>
              </mat-list-item>

              <mat-list-item *ngFor="let item of checkbox_list" class="alternate-row">
                <mat-checkbox
                [disabled]="item.disabled"
                [labelPosition]="item.labelPosition"
                (change)="list_change('groupOne')"
                color="primary"
                [formControlName]="item.formName"
                ></mat-checkbox>
              </mat-list-item>
            </mat-list>
        </div>
        <div formGroupName="groupTwo">
            <mat-list>
              <mat-list-item>
                <mat-checkbox
                color="primary"
                [(indeterminate)]="master_indeterminate.groupTwo"
                (change)="master_change('groupTwo')"
                formControlName="master"
                ><b>2</b></mat-checkbox>
              </mat-list-item>

              <mat-list-item *ngFor="let item of checkbox_list" class="alternate-row">
                <mat-checkbox
                [disabled]="item.disabled"
                [labelPosition]="item.labelPosition"
                (change)="list_change('groupTwo')"
                color="primary"
                [formControlName]="item.formName"
                ></mat-checkbox>
              </mat-list-item>
            </mat-list>
        </div>
        <div formGroupName="groupThree">
          <mat-list>
            <mat-list-item>
              <mat-checkbox
              color="primary"
              [(indeterminate)]="master_indeterminate.groupThree"
              (change)="master_change('groupThree')"
              formControlName="master"
              ><b>3</b></mat-checkbox>
            </mat-list-item>

            <mat-list-item *ngFor="let item of checkbox_list" class="alternate-row">
              <mat-checkbox
              [disabled]="item.disabled"
              [labelPosition]="item.labelPosition"
              (change)="list_change('groupThree')"
              color="primary"
              [formControlName]="item.formName"
              ></mat-checkbox>
            </mat-list-item>
          </mat-list>
        </div>
      </mat-card-content>
    </mat-card>
  </div>
  </form>




