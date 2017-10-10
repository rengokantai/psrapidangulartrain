# psrapidangulartrain

## 6. Data Access with HTTP and RxJS
### 3 Using Http
```
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/throw';
@Injectable()
export class DataService{
  constructor(private http:Http){
  }
  getPostById(id:number):Observable<Post>{
    let url = ''+id;
    return this.http.get(url).map(this.mapPost).catch(this.handleError);
  }
  handleError(res:Response|any):Observable<any>{
    let errorMessage = 'error';
    if (res instanceof Response){
      const body = re.json()||'';
      const err = body.error || JSON.stringify(body);
      errorMessage = ${res.status} - ${res.statusText}${err};
     }
    return Observable.throw(errorMessage);
  }
  mapPost(res: Response):Post{
    let post: Post = res.json();
    return post;
  }
}
```

### 4 Subscribing to Observables
app.component.ts
```
export class AppComponent implement OnInit{
  constructor(private dataService:DataService){
    
  
  ngOnInit(){
    this.dataService.getPostById(1).subscribe((data:Post)=>console.log(data), (err)=>console.log(JSON.stringify(err));
  }
}
```
### 5 Async Pipe
app.component.ts
```
@Component({
  selector: 'app-root',
  template:`<h3 *ngIf="blogPost|async as post">
    {{post.title}}
  </h3>`,
  styleUrls:'[./app.component.css']
})
export class AppComponent implements OnInit{
  blogPost: Observable<Post>;
  constructor(private dataService: DataService){};
  ngOnInit(){
    this.blogPost = this.dataService.getPostById(1);
  }
}
```
Or
```
@Component({
  selector: 'app-root',
  template:`<h3 *ngFor = "let post of blogPosts|async">
    {{post.title}}
  </h3>`,
  styleUrls:'[./app.component.css']
})
export class AppComponent implements OnInit{
  blogPosts: Observable<Post[]>;
  constructor(private dataService: DataService){};
  ngOnInit(){
    this.blogPosts = this.dataService.getPosts();
  }
}
```
### 6 HTTP Actions
data.service.ts
```
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/throw';
@Injectable()
export class DataService{
  constructor(private http:Http){
  }
  getPostById(id:number):Observable<Post>{
    let url = ''+id;
    return this.http.get(url).map(this.mapPost).catch(this.handleError);
  }
  handleError(res:Response|any):Observable<any>{
    let errorMessage = 'error';
    if (res instanceof Response){
      const body = re.json()||'';
      const err = body.error || JSON.stringify(body);
      errorMessage = ${res.status} - ${res.statusText}${err};
     }
    return Observable.throw(errorMessage);
  }
  mapPost(res: Response):Post{
    let post: Post = res.json();
    return post;
  }
  createPost(): Observable<Response>{
    let url = '';
    let post = {userId:1,title:'',body:''};
    return this.http.post(url,post).map(this.mapPost).catch(this.handleError);
  }
}
```

### 7 HTTP Headers and Options
- How do we specify an HTTP Header?
- Import Headers from @angular/http, create a Headers object
```
  createPost(): Observable<Response>{
    let url = '';
    let post = {userId:1,title:'',body:''};
    let headers = new Headers({'Content-Type':'application/json'})
    let options = new RequestOptions({headers:headers});
    return this.http.post(url,post, options).map(this.mapPost).catch(this.handleError);
  }
```
more params:
```
  createPost(): Observable<Response>{
    let url = '';
    let post = {userId:1,title:'',body:''};
    let headers = new Headers({'Content-Type':'application/json'})
    let params = new URLSearchParams();
    params.append('id':'1');
    let options = new RequestOptions({headers:params});
    return this.http.post(url,post, options).map(this.mapPost).catch(this.handleError);
  }
```
### 8 More Observable Operators
do
```
getPostById(id:number):Observable<Post>{
  let url = ''+id;
  return this.http.get(url).do(this.someFunc).map(this.mapPost).catch(this.handleError);
}
```




convert to promise
```
return this.http.get(url).do(this.someFunc).map(this.mapPost).catch(this.handleError).finally(this.func).toPromise();
```
How do we replace an observable with a new one? switchMap
```
return this.get(url).map(this.mapPost).switchMap(()=>Observable.of({id:22}).catch(this..
```


## 7. Forms
Need ```import { Formmodule } from '@angular/forms';```

### 2 Template Driven Form Setup

job-position-form.component.html
```
<form #form="ngForm">
  <input name="title">
  <button type="submit">submit</button>
</form>
{{ form.value|json}}
```

### 3 Form Fields and Data Binding
job-position.ts
```
export class JobPosition{
  title: string;
  isFullTime: boolean;
  department:string;
}
```
job-position-component.ts
```
@Component({
  selector: '',
  templateUrl: './job-position-form.component.html',
  styles:['']
})
exports class JobPositionForm{
  model = new JobPosition();
}
```


job-position-form.component.html  
Question:
- How can we bind to model.isFullTime using a checkbox
```
<form #form="ngForm">
  <input name="title" [(ngModel)]="model.title">
  <input type="checkbox" name="isFullTime" id="fullTime"
  [(ngModel)]="model.isFullTime">
  <label for="isFullTime">Full time</label>
  <button type="submit">submit</button>
</form>
{{ form.value|json }}
```
Question:
- How can we bind to a select tag with options for department
```
<select [(ngModel)]="model.department" name="department">
  <option *ngFor="let d of departments" [value]="d">{{d}}</option>
</select>
```
### 4 State and Validation Classes and Properties
Question:
- What css class is applied once a control loses focus
- ng-touched  

job-position-form.component.css
```
ng-touched:not(form){border:1px solid green;}
```
Question:
- What css class is applied before a control receives focus
- ng-untouched  

job-position-form.component.css
```
ng-untouched:not(form){border:1px solid blue;}
```
Question:
- What css class is applied before a control's value changes?
- ng-pristine  

job-position-form.component.css
```
ng-pristine:not(form){border:1px solid blue;}
```

Question:
- What css class is applied when a control's value is valid/invalid?
- ng-valid/ng-invalid

job-position-form.component.css
```
ng-valid:not(form){border:1px solid blue;}
```
```
<form
</form>
```

Question: __ this part is not easy to remember__
- How can we access properties on NgModel?
- template reference variable

```
<input #title="ngModel" name="title" [(ngModel)]="model.title">
<div [hidden]="title.valid||title.pristine"> show</div>
```

### 5 Submitting Forms
Question:
- How can we hide the submit button until the form is valid?
- Bind to the button's disabled property
```
<button type="submit" [disabled]="form.form.invalid">
  submit
</button>
```
Question:
- How do we call a method on our Component for a submit?
- Bind to the ngSubmit event
- assuming onSubmit is a method of component
```
<form #form="ngForm" (ngSubmit)="onSubmit(form)">
```
## 8. Routing and Navigation
### 2 Router Configuration
```
import {RouterModule,Routes} from '@angular/router';
```
```
<button routerLink="/employee-list routerLinkActive="cssclass"></button>
```

### 3 Route Settings
remember:__pathMatch__  
app.module.ts
```
imports:[
RouterModule.forRoot([
{path:'',redirectTo:'/',pathMatch:'full'}
])
]
```
Question
- unmatch route?
- **
```
{path:'**',component:PageNotFound}
```
### 4 Child Routes
```
RouterModule.forRoot([
  {path:'org-chart',
    component:OrgChartComponent,
    children:[
      {path:'child1',
      component:child1Component
      },{}
    ]
])
```
### 5 The Router Service
```
import { ActivatedRoute, Router } from '@angular/router';
```
Question
- How do we inject ActivatedRoute and Router?
- Add in constructor  

employer-list.component.ts
```
@Component({
  selector:'',
  template:`
  <button (click)="onClick()">click</button>
  `,
  styles:[]
  
})
export class EmployeeListComponent{
  constructor(private activateRoute: Activate,rivate router: Router){}
  onClick(){
  }
}
```
Question
- How do we navigate to employee if it is up one segment in the route tree?
- ```router.navigator('../employee','5')```  
- How to get current url
- ```this.activateRoute.url.subscribe(segments=>console.log(segments.length)); //2```
- ```this.activateRoute.url.subscribe(segments=>console.log(segments[0].path)); //employee2```
- How can we access a parameter's value without subscribing to an Observable?
- ```activateRoute.snapshot```
- Example ```let id= this.activeRoute.anapshot.paramMap.get('id');```
- How can we test for the existence of a parameter using paramMap?
- ```paramMap.has()```
- ```paramMap.keys()```
- How can we subscribe to changing parameters?
- Use ```activatedRoute.paramMap```


### 6 Route Guards
Question
- How do we control if this route can be activated? 
- Use the canActivate property
```
canActivate:[CanActivateGuard]
```
create a service  

can-activate-guard.service.ts
```
export class CanActivateGuard implements CanActivate {
  canActivate(route:ActivatedRouteSnapshot,state:RouterStateSnapshot):Observable<boolean>|Promise<boolean>|booleanP
    return true;
  }
}
```
```
canDeactivate
canActivateChild
```
## 9. Modules, AOT Compilation, and Lazy Loading
### 2 NgModule
Question
- What do we list in the declarations array?
- Components Directives and Pipes
- What do we list in the imports array?
- Modules which are exporting items we need
- What do we list in the providers array?
- Providers of services we want Angular to inject into our application
- How do we set up an NgModule which has a single component for use by others?  

visual-components.module.ts
```
import {NgModule} from '@angular/core';
import {MyVisualComponent} from ./my-visual.component';
@NgModule({
  declarations:[
    MyVisualComponent
  ],
  exports:[
    MyVisualComponent
  ]
})
export class VisualComponentsModule{}
```

