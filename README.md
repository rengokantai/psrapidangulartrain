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
    
  };
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
- How can we bind to ,odel.isFullTime uisng a checkbox
```
<form #form="ngForm">
  <input name="title" [(ngModel)]="model.title">
  <input type="checkbox" name="isFullTime" id="fullTime"
  [(ngModel)]="model.isFullTime">
  <label for="isFullTime">Full time</label>
  <button type="submit">submit</button>
</form>
{{ form.value|json}}
```
