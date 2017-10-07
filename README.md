# psrapidangulartrain

## 6. Data Access with HTTP and RxJS
### 3 Using Http
```
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';
@Injectable()
export class DataService{
  constructor(private http:Http){
  }
  getPostById(id:number):Observable<Post>{
    let url = ''+id;
    return this.http.get(url).map(this.mapPost).catch(this.handleError);
  }
  handleError(res:Response|any):Observable<any>{
    return Observable.throw('error');
  }
  mapPost(res: Response):Post{
    let post: Post = res.json();
    return post;
  }
}
```
