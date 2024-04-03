# section 2. 타입스크립트 기본

# 기본타입

타입스크립트가 자체적으로 제공하는 파일 (내장 타입)

![Untitled](section%202%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20c9558c6601ac4d32bec4ebf762d61d70/Untitled.png)

## 

# 원시타입(primitive type)

---

: 하나의 값만 저장하는 타입

### **number, string, boolean, null, undefined**

<aside>
💡 **Type annotation (타입 주석)**
: 콜론과 함께 어떤 변수의 타입을 정의하는 방식

</aside>

```tsx
//number
let num1: number = 123;
let num2: number = -123;
let nm3 : number = 0.123;
let num4 : number = -9.123;
let num5 : number = Infinity;
let num6 : number = -Infinity;
let num7 : number = NaN;

//string
let str1 : string = "Hello";
let str2 : string = 'World';
let str3 : string = `Hello ${num1}`;

//boolean
let bool1 : boolean = true;
let bool2 : boolean = false;

//null
let null1 : null = null;

//undefined
let unde1 : undefined = undefined;

```

**임시로 다른 type 변수에 null을 넣는 방법**

: tsconfig.json 파일에 `“striceNullChecks” : false` 로 설정! (default : true)

strict 옵션은 strictNullChecks의 하위 옵션 - strict 를 따라간다. 따로 설정도 가능

**➕리터럴 타입**

: 값 자체가 타입이 되는 타입 

- 리터럴 → 값
- 모든 원시 타입 값을 타입으로 사용할 수 있음

# 배열과 튜플

```tsx
//배열
let numArr: number[] = [1, 2, 3];

let strArr: string[] = ["Hello", "World"];

//제네릭 문법으로 정의한 배열
let boolArr: Array<boolean> = [true, false, true];

//배열에 들어가는 요소들의 타입이 다양할 경우 - union type
let multiArr: (string | number)[] = [1, "hello"];

//다차원 배열의 타입을 정의하는 방법
let doubleArr: number[][] = [
  [1, 2, 3],
  [4, 5],
];

//튜플
//자바스크립트에는 없는 타입
//길이와 타입이 고정된 배열
//사실은 배열 

let tup1:[number,number] = [1,2];
// tup1 = [1,2,3]; //에러
// tup1 = ["1","2"]; //에러
let tup2:[number,string,boolean] = [1,"2",true];

tup1.push(1);
tup1.pop();
// 배열 메서드를 사용할 땐 길이 제한이 발동하지 않음!
// 배열이라고 생각하기 떄문에 오류를 발생시키지 않음 --주의

//튜플의 사용
// 인덱스 위치에 따라 넣어야되는 값들이 이미 정해져있고
// 순서를 지키는게 중요할 때 값을 잘못 넣는걸 방지할 수 있다
const users:[string,number][]= [
  ["이정환", 1],
  ["이아무개",2],
  ["김아무개",3],
  ["박아무개",4],
  // [5,"최아무개"],
];
```

# 객체

### TS는 구조적 타입 시스템

= Property Based Type System

: 구조를 기반으로 타입을 정의함

- 객체면 객체 자체(object)가 아니라, 프로퍼티의 타입을 기준으로 타입을 정의함.

↔ 명목적 타입 시스템 : 이름을 기반으로 타입 정의

```tsx
//object 
let user1:object = {
  id:1,
  name: "이정환",
};

//user1.id; // object 타입에 id 프로퍼티가 없다는 에러
//ts의 object type은 객체라는 정보 외에는 아무런 정보가 없어서
// 어떤 프로퍼티가 있는지 알수가 없음!
// 객체긴 한데, 그 이상은 난 몰라! 라는 뜻
//TS -> 구조적 타입 시스템 

//객체 리터럴 타입을 사용하자
let user2:{
  id?:number, // ? : 선택적 프로퍼티 (optional property)
  name: string,
} = {
  id:1,
  name: "이정환",
}
user2.id; // 1

user2 = {
  name : "홍길동",
};

//readonly 프로퍼티 
let config:{
  readonly apiKey:string;
} = {
  apiKey : "My API Key",
};
```

### 선택적 프로퍼티

{변수이름? : 변수타입} → 있어도 되고, 없어도 되는 프로퍼티

### readonly property

값이 변경되면 안되는 변수일 경우 → 변수 이름 앞에 readonly 키워드 붙임

# 타입 별칭과 인덱스 시그니처

### 타입 별칭 (Type Alias)

: 변수를 선언하듯 타입을 별도로 정의하는 방법 

- 같은 스코프 안에 중복 정의 불가

```tsx
//타입 별칭
type User = {
  id:number;
  name : string;
  nickname: string;
  birth:string;
  bio:string;
  location:string;
};

let user: User = {
  id: 1,
  name: "이정환",
  nickname: "winterlood",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "부천시",
};

let user2: User = {
  id: 2,
  name: "홍길동",
  nickname: "winterlood",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "부천시",
};
```

### 인덱스 시그니처

: 객체 타입의 정의를 더 유연하게 하도록 도와주는 문법

: key와 value의 규칙을 기준으로 객체의 type을 정의할 수 있는 문법

- 규칙을 위반하지만 않으면 모든 객체를 허용함!

```tsx
type CountryCodes = {
  [key:string]: string;
};
let countryCodes : CountryCodes= {
  Korea: 'ko',
  USA: 'us',
  UnitedKingdom: "uk"
};

type CountryNumberCodes = {
  [key:string] : number;
};

let countryNumberCodes: CountryNumberCodes= {
  Korea : 410,
  UnitedStates : 840,
  UnitedKingdom : 826,
};
```

- 빈 객체여도 오류를 발생시키지 않음
    - 반드시 포함해야 하는 프로퍼티가 있다면?
        
        ```tsx
        type CountryNumberCodes = {
          [key:string] : number;
          korea : number;
        };
        ```
        
    - 인덱스 시그니처의 value 타입과 새로 추가한 프로퍼티의 value 타입이 호환되지 않으면 오류 발생!
    

# 열거형 타입 Enumerable Type

: *여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입*

**언제 사용할까?**

```tsx
//enum 타입
//여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입
//js에는 없어요

enum Role {
  ADMIN = 0,
  USER = 1,
  GUEST = 2,
}
//숫자를 제거해도 순서대로 0,1,2,.. 
// 10번부터 하고 싶으면 처음만 10 해도 자동으로 11,12,...

const user1 = {
  name: "이정환",
  role: Role.ADMIN,
};

const user2 = {
  name: "홍길동",
  role: Role.USER,
};

const user3 = {
  name: "김아무개",
  role: Role.GUEST,
};

console.log(user1, user2, user3);
//{ name: '이정환', role: 0 } { name: '홍길동', role: 1 } { name: '김아무개', role: 2 }
```

### 숫자형 enum

- 숫자를 자동으로 할당해줌
- 시작하는 숫자를 지정해줄 수도있음

### 문자열 enum

```tsx
enum Language {
  korean = "ko",
  english = "en",
}
```

- 변수 이름 실수 방지

### enum은 컴파일 결과 사라지지 않고 객체가 됨

→ 코드 상에서 마치 값을 사용하듯이 사용할 수 있음

# Any, Unknown

## any

: 특정 변수의 타입을 우리가 확실히 모를 때 사용하는 타입

- 모든 타입을 할당받을 수 있고
- 모든 타입에 값 할당할 수 있다

```tsx
let anyVar = 10;
//anyVar = "string"; -- error!!
// : 타입스크립트는 타입을 지정해주지 않아도,
//초기화하는 값을 기준으로 타입을 추론하기 때문에!

let anyVar1 : any = 10;
let num : number = 10;
num = anyVar1;
//메서드도 제약 없이 사용할 수 있음
```

**문제점**

: 타입 에러가 런타임에 발생함!!

**⇒ 가능한 사용하지 않는다!**

### unknown

: any 타입과 비슷하지만 보다 안전한 타입

- 모든 타입을 할당 받을 수 있고,
- 모든 타입에 저장할 수 있진 않음!

```tsx
let unknownVar : unknown;
 unknownVar = "";
 unknownVar = 10;

 num = unknownVar; //error!
**// 메서드도, 연산도 사용할 수 없다**

//타입정제 시에만 가능
if (typeof unknownVar === "number") {
  num = unknownVar;
}
```

**어떤 값이 들어갈지 모를 떄에는 조금 더 안전한 unknown을 사용하자**

# Void, Never

### void

공허 → 아무것도 없음을 의미하는 타입

- 변수 타입을 void로 설정할 수 있음
- void로 선언한 변수에는 undefined만 담을 수 있음
    
    → tsconfig.json에서 `“strictNullChecks” : false` 하면 null도 예외적으로 할당 가능
    

```tsx
//string을 반환하는 함수
function func1(): string{
  return "hello";
}

//void를 반환하는 함수
function func2(): void{
  console.log("hello");
}

let a: void;
a = undefined;
```

### never

: 불가능을 의미하는 타입

- 함수가 어떠한 값도 반환할 수 없을 때 반환값 타입 정의시 사용
    - void는 반환값이 없는 것 ! never은 불가능한것!
- 변수도 never로 선언가능 → any를 포함한 어느 값도 이 변수에 담을 수 없음
    
     → tsconfig.json에서 `“strictNullChecks” : false` 해도 null값도 할당할 수 없음
    
    ```tsx
    //never
    //존재하지 않는
    // 불가능한 타입
    
    function func3() :never{
      while(true){}
    }
    
    function func4(): never{
      throw new Error();
    }
    
    let a: never;
    ```