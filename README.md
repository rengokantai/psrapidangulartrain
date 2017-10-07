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
