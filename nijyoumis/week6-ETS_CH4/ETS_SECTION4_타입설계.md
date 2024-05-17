# week6

# item 28 : 유효한 상태만 표현하는 타입을 지향하기

1. 웹 페이지를 만든다고 가정해보자.

다음과 같은 상태가 있다.

```jsx
interface State{
	pageText : string;
	isLoading : boolean;
	error?: string;
}
```

- 상태 값의 두 가지 속성이 동시에 정보가 부족하거나 (요청이 실패한건지 , 로딩중인지)
- 두 가지 속성이 충돌 ( 오류이면서 동시에 로딩중일 수 있음)

⇒ 페이지 render, 전환 시 state를 나누는데 문제가 생길 수 있다

```tsx
interface RequestPending{
	state: 'pending';
}
interface RequestError{
	state: 'errer';
	error : string;
}
interface RequestSuccess {
	state : 'ok';
	pageText: string;
}
type RequestState = RequestPending | RequestError | RequestSuccess;

interface State{
	currentPage : string;
	requests : {[page:string]: RequestState};
}
```

→ 네트워크 요청 과정 각각의 상태를 명시적으로 모델링하는 tagged union 사용

무효한 상태를 허용하지 않도록 개선되었다.

1. 에어버스 330

기장 / 부기장을 위한 두 개의 분리된 제어 세트

```tsx
interface CockpitControls {
	leftSideStick : number;
	rightSideStick : number;
}

function getStickSet(controls : CockpitControls{
	const {leftSideStic, rightSideStick} = controls;
	if (leftSideStick ===0){
		return rightSideStick;
	}else if (rightSideStick ===0){
		return leftSideStick; // 기장이 중립일 때 - 부기장 stick
	}
	if(Math.abs(leftSideStick - rightSideStick <5){
		return (leftSideStick + rightSideStick ) /2 //둘다 중립이 아닐 때
	}
}
```

⇒ 둘의 차이가 많이나면 - 에어버스는 아무 일도 하지 않게 됨

```tsx
// 두 개의 스틱을 기계적으로 연결
interface CockpitControls{
	stickAngle : nubmer;
}
```

<aside>
❗ 결론 : 유효한 상태를 표현하는 값만  허용하여 코드를 작성하자!

</aside>

# item 29 : 사용할 때는 너그럽게, 생성할 때는 엄격하게

함수의 매개변수는 타입의 범위가 넓어도 되지만, 결과를 반환할 때는 타입의 범위가 더 구체적이어야 한다.

3D 매핑 API 예시

- 카메라 위치를 지정하고, 경계 박스의 뷰포트를 계산하는 방법을 제공한다

```tsx
declare function setCamera (camera: CameraOptions ) :void;
declare function viewportForBoudns (bounds : LngLatBounds ) : CameraOptions;

interface CameraOptions{
	center?: LngLat;
	zoom ?: number;
	bearing ?: number;
	pitch?: number;
}
type LngLat =
{lng : number; lat: number} |
{lon:number; lat: number) |
[number, number];

type LngLatBounds =
{northeast : LngLat, southwest : LngLat }|
[LngLat, LngLat] }
[number,number,nubmer,number];

// 매개변수의 타입이 자유로움
```

```tsx
// lat,lng 속성이 없고 zoom 속성만 있을 때 
const {center: {lat,lng}, zoom} =camera ;
//'lat'속성이 없습니다, 'lng'속성이 없습니다

zoom ; // 타입이 number | undefined
```

문제는 타입 선언이 사용될 때 뿐만 아니라 만들어질 때에도 너무 자유롭다는 것!

매개변수의 타입의 범위가 넓을수록 반환 타입이 엄격하다

유니온 타입의  요소별 분기를 위한 한 가지 방법은, 좌표를 위한 기본 형식을 구분하는 것. 

- 배열 / 배열같은 것의 구분을 위해 LngLat과 LngLatLike를 구분할 수 있다
- 완전하게 정의된 타입 / 부분적으로 정의된 타입으로 버전을 구분할 수 있다

```tsx
interface LngLat {lng : number ; lat : number;};
type LngLatLike = LngLat | {lon: number ; lat : number}  | [number,number];
interface Camera{
	center: LngLat;
	zoom : number;
	bearing : number;
	pitch: number;
}
interface CameraOptions extends Omit<Partial<Camera>,'center'>
	center?: LngLatLike;
}
type LnglatBounds = 
{northeast : LngLatLike, southwest : LngLatLike}|
[LngLatLike, LngLatLike]  |
[number,number,nubmer,number];
```

**요약**

> 보통 매개변수 타입은 반환 타입에 비해 범위가 넓은 경향이 있다. 선택적 속성과 유니온 타입은 반환 타입보다 매개변수 타입에 더 일반적이다.
> 

> 매개변수와 반환 타입의 재사용을 위해서 기본형태(반환 타입) 와 느슨한 형태(매개변수 타입)를 도입하는 것이 좋다.
> 

# item 30 : 문서에 타입 정보를 쓰지 않기

- 코드와 주석은 동기화 되지 않는다
    - 타입 정보에 모순이 생길 수 있다
- 특정 매개변수를 설명하고 싶다면 JSDoc의 @param 구문을 사용하면 된다.
- 값을 변경하지 않는다고 설명하는 주석도 좋지 않다.
- 매개변수를 변경하지 않는다고 설명하는 주석도 사용하지 말자.
    - 대신, readonly를 사용하여 타입스크립트가 규칙을 강제할 수 있게 하자
- 타입이 명확하지 않은 경우에는 변수명에 단위 정보를 포함하는 것을 고려하는 것이 좋다(timeMs, temperatureC)

# item 31 : 타입 주변에 null값 배치하기

**값이 전부 null이거나 전부 null이 아닌 경우로 분명히 구분된다면 타입에 null을 추가하는 방식으로 모델링할 수 있다.**

```tsx
function extent(nums : number[]){
	let min, max;
	for (const num of nums){
		if (!min){
			min = num;
			max = num;
		}else{
			min = Math.min(min, num);
			max = Math.max(max, num);
						// strictNullChecks를 키면
						// 'number|undefined 형식의 인수는 'number' 형식의 매개변수에 할당될 수 없다
						//는 에러가 생김
						// min에서만 undefined를 제외했기 때문!
		}
	}
	return [min,max];
}
```

- 최솟값이나 최댓값이 0인 경우, 값이 덧씌워짐
- nums 배열이 비어있다면 함수는 [undefined, undefined] 를 반환

min, max는 동시에 초기화되지만, 이러한 정보는 타입시스템에서 표현할 수 없다.

min, max를 한 객체 안에 넣고 null이거나 null이 아니게 하면 된다

```tsx
function extent(nums : number[]){
	let result : [number, number] |null = null; //타입에 null을 추가
	for (const num of nums){
		if (!result){
			result = [num,num];
		}else
			result = [Math.min(min, result[0], Math.max(max, result[1]);
		}
	}
	return result; // 반환 타입이 [number,number] | null 
}

const [min,max] = extent([0,1,2]) ! //null 아님 단언을 사용해서 min max를 얻을 수 있다
```

⇒ extent의 결과 값으로 단일 객체를 사용함으로 써 설계 개선,

⇒ 타입스크립트가 null 값 사이의 관계를 이해할 수 있도록 했음

**null과 null이 아닌 값을 섞어서 사용하면 클래스에서도 문제가 생긴다**

상태가 모두 준비가 되었을 때 생성하여 null이 아니게 해야함

```tsx
class UserPosts{
	user : UserInfo | null;
	posts : Post[] | null;
	
	constructor(){
		this.user = null;
		this.posts = null;
	}
	async init (userId : string){
		return Promise.all([
			async () => this.user = await fetchUser(userId),
			async () => this.posts = await fetchPostsForUser(userId)
		]);
	}
	getUserName(){
		...?
	}
}
```

두 번의 요청이 로드되는 동안 user 과 posts를 null상태이다.

- 둘다 null
- 둘 중 하나만 null
- 둘 다 null 아닌

4가지 상태가 존재한다 → 속성값의 불확실성이 모든 메서드에 나쁜 영향을 미침

```tsx
class UserPosts{
	user : UserInfo;
	posts : Post[];
	
	constructor(){
		this.user = null;
		this.posts = null;
	}
	static async init (userId : string) : Promise<UserPosts> {
		const [user,posts] = await Promise.all([
			fetchUser(userId),
			fetchPostsForUser(userId)
		]);
		return new UserPosts(user,posts);
	}
	getUserName(){
		return this.user.name;
	}
}
```

- UserPosts 클래스는 완전히 null이 아니게 됨
- null인 경우가 필요하다면 Promise로 바꾸면 안됨!
    - promise는 데이터를 로드하는 코드를 단순하게 해주지만, 데이터를 사용하는 클래스에서는 코드가 복잡해지게 하기도 한다.

**요약**

> 한 값의 null 여부가 다른 값의 null 여부에 암시적으로 관련되도록 설계하면 안된다
> 

> API 작성 시에 반환 타입을 큰 객체로 만들고 반환 타입 전체가 NULL이거나 null이 아니게 만들어야 한다. 사람과 타입 체커 모두에게 명료한 코드를 작성한다.
> 

> 클래스를 만들 때에는 필요한 모든 값이 준비되었을 때 생성하여 null이 존재하지 않도록 하는 것이 좋다
> 

> strictNullChecks를 설정하면 코드에 많은 오류가 표시되겠지만, null값과 관련된 문제점을 찾아낼 수 있기 때문에 반드시 필요하다
> 

# item 32 : 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

벡터를 그리는 프로그램을 작성중이다.

```tsx
interface Layer {
	layout : FillLayout | LineLayout | PointLayout;
	paint: FillPaint | LinePaint | PointPaint;
}
```

- LineLayout(직선) 이면서 FillPaint 인건 말이 안됨

⇒ 불가능한 조합을 허용하지 않도록 **타입 계층을 분리된 인터페이스로 작성**해야한다.

```tsx
interface FillLayer{
	layout : FillLayout;
	paint : FillPaint;
}
interface LineLayer {
	layout : LineLayout ;
	paint : LinePaint ;
}
interface PointLayer {
	layout : PointLayout ;
	paint : PointPaint;
}
type Laer = FillLayer | LineLayer | PointLayer ;
```

태그된 union 도 마찬가지로 interface의 union을 사용하는 것이 바람직

```tsx
interface Layer {
	type : 'fill' | 'Line' | 'Point'
	layout : FillLayout | LineLayout | PointLayout;
	paint: FillPaint | LinePaint | PointPaint;
}

//대신에 interface 분리 후 , union

interface FillLayer{
	type : 'fill';
	layout : FillLayout;
	paint : FillPaint;
}
interface LineLayer {
	type : 'Line';
	layout : LineLayout ;
	paint : LinePaint ;
}
interface PointLayer {
	type : 'Point';
	layout : PointLayout ;
	paint : PointPaint;
}
type Laer = FillLayer | LineLayer | PointLayer ;
```

어떤 데이터를 태그된 유니온으로 표현할 수 있다면, 그렇게 하는 것이 좋다!

**여러 개의 선택적 필드가 동시에 값이 있거나 동시에 undefined인 경우도 태그된 유니온 패턴이 적절하다**

```tsx
interface Person {
	name : string;
	placeOfBirth : string;
	dateOfBirth : Date;
}

// placeOfBirth와 dateOfBirth가 동시에 있거나, 없다면
// 두 개의 속성을 하나의 객체를 더 나은 설계이다
interface Person {
	name : string;
	birth?: {
		place : string;
		date : Date;
	}
}
// Person 객체를 매개변수로 받는 함수에서는 birth 하나만 체크하면 된다
```

타입 구조를 손댈 수 없는 상황이라면, 인터페이스의 유니온을 사용하자

요약

> 유니온 타입의 속성을 여러 개 가지는 인터페이스에서는 속성 간의 관계가 분명하지 않기 때문에 실수가 자주 발생하므로 주의해야 한다.
> 

> 유니온의 인터페이스보다 인터페이스의 유니온이 더 정확하고 타입스크립트가 이해하기도 좋다
> 

> 타입스크립트가 제어 흐름을 분석할 수 있도록 타입에 태그를 넣는 것을 고려해야 한다. 태그된 유니온은 타입스크립트와 잘 맞기 때문에 자주 볼 수 있는 패턴이다.
> 

# item 33 : string 타입보다 더 구체적인 타입 사용하기

```tsx
type RecordingType = 'studio' | 'live';

interface Album = {
	artist : string;
	title : string;
	releaseDate : Date; // 'YYYY-MM-DD' 만 허용
	recordingType : RecordingType; // Studio - error !
}
```

→ 이렇게 작성했을 때 장점

1. **타입을 명시적으로 정의함으로써 다른 곳으로 값이 전달되어도 타입 정보가 유지된다.**
    
    ex) 특정 레코딩 타입의 앨범을 찾는 함수를 작성할 때,
    
    ```tsx
    function getAlbumsofType(recordingType:string) : Album[]{
    	//
    	}
    ```
    
    함수를 호출하는 곳에서 string 이어야 한다는 것 외에는 다른 정보가 없음 
    
2. **타입을 명시적으로 정의하고 해당 타입의 의미를 설명하는 주석을 붙여 넣을 수 있다**
    
    getAlbumsOfType이 받는 매개 변수를 string 대신에 RecordingType으로 바꾸면 편집기에서 주석을 볼 수 있다
    
    ```tsx
    function getAlbumsofType(recordingType:RecordingType) : Album[]{
    	//
    	}
    ```
    
3. **keyof 연산자로 세밀하개 객체의 속성 체크가 가능하다**
    
    어떤 배열에서 한 필드의 값만 추출하는 함수가 있을 때, 
    
    ```tsx
    function pluck<T> (records:T[], key: keyof T): T[key of T][]
    ```
    
    T[key of T] 는 T 객체 내의 가능한 모든 값의 타입이다.
    
    → key의 값으로 문자열을 넣게 되면 범위가 너무 넓다.
    
    ```tsx
    const releaseDats = pluck (albums, 'releaseDate'); // 타입이 (string |Date) [] 
    //Date[] 이어야 적절하다
    ```
    
    ⇒ keyof T의 부분집합으로 두 번째 제너릭 매개변수를 도입해야한다.
    
    ```tsx
    function pluck<T, K extends key of T> (records : T[], key: K) : T[K][]{
    	return records.map(r=>r[key]);
    } // 해결!
    ```
    

string은 any와 비슷한 문제를 가지고 있다. 잘못 사용하게 되면 무효한 값을 허용하고 타입 간의 관계도 감추어버린다. string의 부분집합을 정의할 수 있는 기능을 통해 안전성을 높이자.

**요약**

> ‘문자열을 남발하여 선언된’ 코드를 피하자. 모든 문자열을 할당할 수 있는 string 타입보다 더 구체적인 타입을 사용하는 것이 좋다
> 

> 변수의 범위를 보다 정확하게 표현하고 싶다면 string 타입보다는 문자열 리터럴 타입의 유니온을 사용하자. 타입 체크를 더 엄격히 할 수 있고, 생산성을 향상시킬 수 있다
> 

> 객체의 속성 이름을 함수 매개변수로 받을 때는 string 보다 keyof T를 사용하는 것이 좋다
> 

# item 34 : 부정확한 타입보다는 미완성 타입을 사용하기

타입 선언의 정밀도를 높이는 일에는 주의를 기울어야 한다. 실수가 발생하기 쉽고 잘못된 타입은 차라리 타입이 없는 것보다 못할 수 있다.

GeoJSON 형식의 타입 선언을 작성한다고 가정해보자.

→ 각각 다른 형태의 좌표 배열을 가지는 타입 중 하나가 될 수 있음

```tsx
interface Point{
	type : 'Point';
	coordinates : number[];
}
interface LineString{
	type: 'LineString';
	coordinates : number[][];
}
interface Polygon{
	type: 'Polygon'
	coordinates : number[][][];
}
type Geometry = Point | LineString | Polygon;
```

좌표가 number[] ⇒ 추상적이어서 [위도, 경도] 튜플 타입으로 정밀하게 선언해버리면

- 세 번째 요소인 고도가 있을 수 있고 , 또 다른 정보가 있을 수 있음

타입이 구체적으로 정제된다고 해서 정확도가 무조건 올라가지는 않는다

**요약**

> 타입 안정성에서 불쾌한 골짜기는 피해야 한다. 타입이 없는 것보다 잘못된 게 더 나쁘다
> 

> 정확하게 타입을 모델링할 수 없다면, 부정확하게 모델링하지 말아야한다. 또한 any와 unknown을 구분해서 사용해야 한다.
> 

# item 35 : 데이터가 아닌, API와 명세를 보고 타입 만들기

명세를 참고해 타입을 생성하면 실수를 줄일 수 있다.

**요약**

> 코드의 구석구석까지 타입 안전성을 얻기 위해 API 또는 데이터 형식에 대한 타입 생성을 고려해야 한다.
> 

> 데이터에 드러나지 않는 예외적인 경우들이 문제가 될 수 있기 때문에 데이터보다는 명세로부터 코드를 생성하는 것이 좋다.
> 

# item 36 : 해당 분야의 용어로 타입 이름 짓기 (1)

```tsx
interface Animal{
	name : string;
	endangered : boolean;
	habitat : string;
}
```

⇒ 문제 4가지

1. name은 너무 일반적인 용어다. 동물의 학명인지, 일반적인 명칭인지 알 수 없다
2. endangered 속성이 true인 것이 멸종된 동물인지 불명확하다.
3. habitat 속성이 너무 범위가 넓은 string이다. 서식지라는 뜻이 모호하다.
4. 객체의 이름과 속성의 name이 다른 의도로 사용되는지 불분명하다.

```tsx
interface Animal {
	commonName : string;
	genus : string;
	species : string;
	status : ConservationStatus;
	climates : KoppenClimate [];
}
```

자체적으로 용어를 만들어내지말고, 해당 분야에 이미 존재하는 용어를 사용하자.

**타입, 속성, 변수에 이름을 붙일 때 명심해야 할 세 가지 규칙**

1. 동일한 의미를 나타낼 때는 같은 용어를 사용해야 한다.
2. data, info, thing, object, item, entity 같은 모호하고 의미 없는 이름은 피해야 한다.
3. 이름을 지을 때는 포함된 내용이나 계산 방식이 아니라 데이터 자체가 무엇인지를 고려해야한다.
    
    INodeList보다는 Directory가 더 의미있는 이름이다. 구현의 측면이 아니라 개념적인 측면에서 더 좋은 이름이다.
    

**요약**

> 가독성을 높이고, 추상화 수준을 올리기 위해 해당 분야의 용어를 사용해야 한다.
> 

> 같은 의미에 다른 이름을 붙이면 안된다. 특별한 의미가 있을 때만 용어를 구분하자.
> 

# item 37 : 공식 명칭에는 상표를 붙이기

```tsx
interface Vector2D{
	x: number;
	y: number;
}
function calculateNorm(p:Vector2D){
	return Math.sqrt(p.x*p.x +p.y*p.y);
}
calculateNorm({x:3, y:4});
const vec3D = {x:3, y:4, z:1};
calculateNorm(vec3D); // 5로 정상작동
```

3차원 벡터를 허용하지 않게 하려면 공식 명칭 (nominal typing)을 사용하면 된다

공식 명칭을 사용하는 것은, 타입이 아니라 값의 관점에서 Vector2D라고 말하는 것이다.

공식 명칭 개념을 타입스크립트에서 흉내내려면 ‘상표(brand)’ 를 붙이면 된다.

```tsx
interface Vector2D{
	_brand : '2d';
	x: number;
	y: number;
}

function vec2D(x:number, y:number) : Vector2D{
	return {x,y,_brand:'2d'};
}

function calculateNorm(p:Vector2D){
	return Math.sqrt(p.x*p.x + p.y*p.y);
}
calculateNorm(vec2D(3,4));
calculateNorm(vec3D) ; // brand 속성이 ... 형식에 없습니다
```

⇒ 상표 _brand를 사용해서 Vector2D 타입만 받는 것을 보장한다.

- 상표 기법은 타입 시스템에서 동작하지만 런타임에 상표를 검사하는 것과 동일한 효과를 얻을 수 있다.
- 타입 시스템이기 때문에 런타임 오버헤드를 없앨 수 있고 추가 속성을 붙일 수 없는 string이나 number같은 내장 타입도 상표화할 수 있다.

**절대 경로를 사용해 파일 시스템에 접근하는 함수를 가정해보자.** 

- 런타임에는 절대 경로 (’/’) 로 시작하는지 체크하기 쉽지만, 타입 시스템에서는 절대 경로를 판단하기 어렵기 때문에 상표 기법을 사용한다.
    
    ```tsx
    type AbsolutePath = string & {_brand : 'abs'};
    function listAbsolutePaht(path : Absolutepath){
    	...
    }
    function isAbsolutepath(path:string) : path is Absolutepath
    	return path.startWith('/');
    }
    ```
    
- 만약 path 값이 절대 경로와 상대 경로  둘 다 될 수 있다면, 타입을 정제해주는 타입 가드를 사용해서 오류를 방지할 수 있다
    - if체크로 타입에 따라 분기
    - 단언문은 지양해야하기 때문에 단언문을 쓰지 않고 Absolutepath 타입을 얻기 위한 유일한 방법은 AbsolutePath 타입을 매개변수로 받거나 타입이 맞는지 체크하는 것뿐이다.

**상표 기법은 타입 시스템 내에서 표현할 수 업슨 수많은 속성들을 모델링하는데 사용되기도 한다.** 

ex) 이진 검색 - 이미 정렬된 상태를 가정.

타입스크립트 타입 시스템에서는 목록이 정렬되어 있다는 의도를 표현하기 어렵다. → 상표 기법 사용

```tsx
type SortedList<T> = T[] & {_brand : 'sorted'};
function isSorted<T>(xs:T[]): xs is SortedList<T> {
	.....
	
}
function binarySearch<T>(xs: SortedList<T>, x: T):boolean{}
```

binarySearch를 호출하려면, 정렬되었다는 상표가 붙은 SortedList 타입의 값을 사용하거나, isSorted를 호출하여 정렬되었음을 증명해야 한다.

**number 타입에도 상표를 붙일 수 있다.**

```tsx
type Meters = number & {_brand: 'meters'};
type Seconds = number & {_brand: 'seconds'};

const meters = (m:number) => m as Meters;
const seconds = (s: number) => s as Seconds;

const oneKm = meters(1000); // 타입이 Meters
```

number 타입에 상표를 붙여도 산술 연산 후에는 상표가 없어지기 때문에 실제로 사용하기에는 무리가 있음.

그러나 코드에 여러 단위가 혼합되어 있는 경우, 숫자의 단위를 문서화하는 괜찮은 방법일 수 있다.

**요약**

> 타입스크립트는 구조적 타이핑을 사용하기 때문에, 값을 세밀하게 구분하지 못하는 경우가 있다. 값을 구분하기 위해 공식 명칭이 필요하다면 상표를 붙이는 것을 고려해야한다.
> 

> 상표 기법은 타입 시스템에서 동작하지만 런타임에 상표를 검사하는 것과 동일한 효과를 얻을 수 있다.
>