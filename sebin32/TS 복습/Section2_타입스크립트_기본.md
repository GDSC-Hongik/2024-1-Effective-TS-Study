# Section2. 타입스크립트 기본

## 기본 타입

기본 타입 : 타입스크립트가 자체적으로 제공하는 타입

![Untitled](Section2%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%204801a6a81b414492b8f3ffe4aa824059/Untitled.png)

원시 타입 (Primitive Type) : 하나의 값만 저장하는 타입

number - 정수, 소수, 무한, NaN 

```tsx
//number
let num1: number = 123; 
num1 = 'hello' //에디터에서 오류 메시지 보여줌
num1.toUpperCase() //문자열 메서드의 경우도 마찬가지로 사용 불
```

`<변수명> : <type>` - type annotation (타입 주석)

string - 문자열, 백틱 문자열, 템플릿 리터럴 문자열 

```tsx
let str1: string = "hello";
let str2: string = 'hello';
let str3: string = `hello ${num1}`;

str1 = 123; //오류 발생
str1.toFixed() //숫자 메서드라서 오류 발생
```

boolean - 참, 거짓

```tsx
let bool1: boolean = true;
let bool2: boolean = false;
```

null - null 이외의 값 불허

```tsx
let null1: null = null;
null1 = 1; //오류 메시지
```

undefined 

```tsx
let unde1: undefined = undefined;

```

literal - 딱 하나의 값만 포함하는 리터럴 타입

```tsx
let numA: 10 = 10;
numA = 12; //오류 발생 , 오직 10만 할당할 수 있음
```

<aside>
⭐ What if > 변수를 선언하고 잠시 null값을 넣어두고 싶은 상황이라면?

 - tsconfig.json 에서 `“StrictNullChecks” : false` 설정

⇒  `null` 값을 엄격하게 검사하지 않겠다는 설정

strict option이 strictNullChecks option의 상위 option이라서 strictNullChecks Option을 명시하지 않은 경우 대개 strict option에 종속되는데,  명시하는 경우에는 별개로 동작 

</aside>

## 배열과 튜플

배열 

```tsx
let numArr: number[] = [1,2,3]; 
//배열의 원소가 number인 배열 numArr

let strArr: string[] = ['hello','hi'];
//배열의 원소가 string인 배열 strArr

let boolArr: Array<boolean> = [true,false,true];
//배열의 원소가 boolean인 배열 boolArr
//제네릭 방식으로 타입 명시

//배열에 들어갈 원소의 타입이 다양할 경우
let multiArr: (string | number)[] = [1, 'hello'];
//string이나 number타입의 원소를 갖는 배열 multiArr

//다차원 배열의 타입 정의
let doubleArr: number[][] = [
	[1,2,3],
	[4,5],
];
//number type의 배열을 담는 배열 doubleArr
```

튜플 - js에는 없고 ts에만 있는 type , 길이와 타입이 고정된 배열

```tsx
let tup1: [number,number] = [1,2];
//요소의 길이와 타입을 고정하는 튜플 tup1

tup1 = [1,2,3]; //길이에서 오류 발생
tup1 = ['1','2'];  //타입에서 오류 발생

let tup2: [number,string,boolean] = [1,'2',true];
//
```

튜플은 별도로 존재하는 자료형이라고 생각하기에는 무리가 있음 

실제로 compile 결과를 확인해도 배열의 형태로 compile됨. 

⇒ push method, pop method과 같은 배열 메서드 모두 사용 가능하므로 주의가 필요함 (길이 면에서)

튜플이 유용한 경우 - 인덱스의 위치에 따라 넣어야 하는 값이 정해져 있고, 순서가 중요한 경우 

```tsx
const users: [string, number][] = [
	['이정환',1],
	['이아무개',2],
	['김아무개',3],
	['박아무개',4],
	[5,'최아무개'],  //해당 값에서 오류 발생
];
```

## 객체

```tsx
let user: object ={
	id: 1,
	name: '이정환',
};

user.id; //object 타입으로 명시한 객체에 점 표기법으로 접근하는 경우 오류 발생
//ts의 object라는 타입은 값이 객체라는 정보 외에는 아무런 정보가 없음. 
//=> **객체 리터럴**을 사용해서 정의해야 함. 

let user: {
	id : number;
	namd: string;
} = {
	id: 1,
	name: 'jeonghwan',
};

user.id; //id의 type이 number인 것까지 ts가 인지

let dog: {
	name : string;
	color : string;
} = {
	name: 'dodo',
	color: 'brown',
};
```

구조적 타입 시스템 - 객체의 구조를 기준으로 type 정의

= property based type system - property를 기준으로 type을 결정하는 시스템 

↔ 명목적 타입 시스템 : 객체의 이름을 기준으로 타입을 정의 ex> c언어, java

특정 property가 없어도 되도록 객체를 정의하는 방법

```tsx
let user: {
	id**?**: number;   
	//해당 property가 있어도 되고, 없어도 되지만 있을 경우 타입이 number
	//optional property라고 
	name: string;
} = {
	id: 1,
	name: 'jeonghwan',
};
```

특정 property의 값을 변경할 수 없도록 객체를 정의하는 방법

```tsx
let config: {
	readonly apiKey: string;
} = {
	apiKey: 'My API Key',
};

config.apiKey = 'hacked'; // 오류 발생 -> readonly 설정 때문에 
```

## 타입 별칭과 인덱스 시그니처

타입 별칭

```tsx
let user1 = {
	id: number;
	name: string;
	nickname: string;
	birth: string,
	bio: string,
	location: string;
} = {
	id: 1,
	name: 'sebin',
	nickname: 'sbn',
	birth: '1999.03.02',
	bio: '안녕하세요',
	location: '성남시',
};

let user2 = {
	id: number;
	name: string;
	nickname: string;
	birth: string,
	bio: string,
	location: string;
} = {
	id: 2,
	name: 'daeun',
	nickname: 'dn',
	birth: '2000.02.03',
	bio: 'daeunarea',
	location: '복수동',
};
//위와 같이 type이 겹치는 변수를 2개 이상 선언하는 경우 아래와 같이 컴팩트한 코드 작성

**type User = {
	id: number;
	name: string;
	nickname: string;
	birth: string,
	bio: string,
	location: string;
};**

let user1: User = {
	id: 1,
	name: 'sebin',
	nickname: 'sbn',
	birth: '1999.03.02',
	bio: '안녕하세요',
	location: '성남시',
};

let user2: User = {
	id: 2,
	name: 'daeun',
	nickname: 'dn',
	birth: '2000.02.03',
	bio: 'daeunarea',
	location: '복수동',
};
```

주의 할 점 

타입 별칭은 동일한 scope에 중복된 이름으로 type 별칭 선언 시 오류 발생 

마치 변수의 이름 같음 따라서 같은 스콥 안에서는 다른 이름으로 선언해야 함

그리고, js로 컴파일되는 과정에서 타입 관련 코드가 모두 제거되므로 타입 별칭의 타입들도 제거 됨 

인덱스 시그니처

```tsx
type CountryCodes = {
	[key: string]: string;
};

let coutnryCodes: CountryCodes = {
	Korea: 'ko',
	UnitedStates: 'us',
	UnitedKingdom; 'uk',
};

type CountryNumberCodes = {
	[key: string]: number;
}

let countryNumberCodes: CountryNumberCodes = {
	Korea: 410,
	UnitedStates: 840,
	UnitedKingdom: 826,
};

let countryNumberCodes1: CountryNumberCodes = {};
//해당 케이스에 대해 에러x ∵규칙을 위반하지 않아서

///////////////////아래의 경우 다른 케이스////////////////////////////
type CountryNumberCodes = {
	[key: string]: number;
	Korea: number;
};
//이 경우에는 반드시 Korea: number 값이 존재해야 에러 발생 x 
//그러나 제약 사항 존재

type CountryNumberCodes = {
	[key: string]: number;
	Korea: string;        //해당 부분에서 에러 발생
};
//추가적인 property의 value 타입이 반드시 인덱스 시그니처의 value 타입과 일치 or 호환되어야 함
```

## 열거형 타입 Enum 타입

Enum 타입 - 여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입, 오직 ts만 

사용해야 하는 이유 - 프로그래머가 헷갈리지 않고 사용할 수 있음

```tsx
enum Role { //type 별칭과 다르게 등호 존재 x 
	ADMIN = O,   //숫자 할당을 제거해도 0,1,2의 순서대로 할당 됨 ADMIN에 10을 할당하면 10,11,12가 할당되고, 
	USER = 1,    // USER에 10을 할당하면 0,10,11이 할당 됨.
	GUEST = 2,   // 문자열 할당도 가능 
};

const user1 = {
	name: '이정환',
	role: Role.ADMIN //관리자
};

const user2 = {
	name: '홍길동',
	role: Role.USER  //일반 유저
};

const user3 = {
	name: '아무개',
	role: Role.GUEST //게스
};

console.log(user1,user2,user3); 
// { name: 이정환, role: 0}, { name: 홍길동, role: 1}, { name: 아무개, role: 2}
```

컴파일 되면 타입 관련된 것들은 사라졌는데, Enum은 사라지지 않고, JS의 객체로 변환되기 때문에 사라지지 않는다. 

## Any와 Unknown 타입

Any - 특정 변수의 타입을 우리가 확실히 모를 때 사용 

타입 검사를 다 통과하는 치트 키 너낌~   

```tsx
let anyVar = 10;
anyVar = 'hello'; //오류 발생 -> 아래와 같이 수정하면 해결 ㄱㄴ

let anyVar: any = 10;
anyVar = 'hello';
anyVar = true;
anyVar = {};
anyVar = () => {};

anyVar.toUpperCase();   //runtime에서 에러 발생
anyVAR.toFixed();

let num: number = 10;
num = anyVar; // any는 모든 type의 변수에 할당 가능 => type 오류 발생 x 

```

Unknown - 

```tsx
let unknownVar: unknown;

unknownVar = '';
unknownVar = 1;
unknownVar = () => {};

num = unknownVar // unknown 타입은 number 타입에 할당될 수 없음 
//즉, 다른 타입에 할당은 불가능함 any와 차이

unknownVar.toUpperCase(); // unknown 형식은 메서드 사용 불가, 연산도 불가

if ( typeof unknownVar ==='number' ) {  //타입 정제를 거쳐야만 할당, 연산 가 
	num = unknownVar;
};
```

## Void 타입과 Never 타입

void - 아무것도 없음을 의미하는 타입 

```tsx
function func1(): string {
	return "hello";
};

function func2(): void {
	console.log('hello');
};

let a: void;
a = 1;       //에러 발생 -> void 타입은 아무 것도 할당할 수 없음
a = 'hello'; //에러 발생 -> void 타입은 아무 것도 할당할 수 없음
a = undefined; //에러 발생 x 
a = null; // tsconfig.json에서 strictNullChecks를 false로 설정한 경우 가능

//굳이 void를 쓰는 이유
 function func3(): undefined {   
	console.log('hello');
//해당 경우에 에러 발생 return undefined;까지 작성해야 해결 가능 
};

function func4(): null {   
	console.log('hello');
//해당 경우도 에러 발생 return ;까지 작성해야 해결 가능 
};

```

Never - 존재하지 않는 불가능한 타입

```tsx
function func3(): never {
	while (true) {}   //정상적인 종료를 기대할 수 없고, 반환하는 것 자체가 모순일 때
};

function func4(): never {
	throw new Error();   //프로그램이 중지될 것이기 때문에
};

let a: never;
a = 1;
a = '';
a = {};
a = undefined;
a = null;
let anyVar: any;
a = anyVar;
//위의 모든 case가 error case
```