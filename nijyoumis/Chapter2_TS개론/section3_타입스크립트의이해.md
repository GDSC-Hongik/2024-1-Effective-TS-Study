# section 3. 타입스크립트 이해하기

## 타입스크립트 이해하기

1. 어떤 기준으로 타입을 정의하는지
2. 어떤 기준으로 타입간의 관계를 정의하는지
3. 어떤 기준으로 타입의 오류를 검사하는지

## 타입은 집합이다

타입 = 동일한 속성을 갖는 값들을 모아둔 집합

number literal type은 number type의 부분집합

- 슈퍼타입(부모타입) : 다른 타입을 포함하는 더 큰 타입
- 서브타입(자식타입) : 다른 타입에 포함되는 타입

타입이란 값들을 포함하는 집합이며, 따라서 서로 타입들끼리 부모와 자식 관계를 맺으며 결국 모든 타입들의 관계를 놓고 보면 타입 계층도로 만들어서 표현할 수 있다. 

### 타입 호환성

: 어떤 타입을 다른 타입으로 취급해도 괜찮은지 판단하는 것

Number type → Number literal type (x)

Number literal type → Number Type (o)

**서브타입의 값을 슈퍼타입에 언제나 할당할 수 있지만, <업캐스팅>**

**슈퍼타입의 값을 서브타입에 대부분 할당할 수 없다! <다운캐스팅>**

## 타입 계층도와 함께 기본타입의 호환성 살펴보기

![Untitled](section%203%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20aa07e1a3b3ae4da7bd5f0ed1c90f66b1/Untitled.png)

![Untitled](section%203%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20aa07e1a3b3ae4da7bd5f0ed1c90f66b1/Untitled%201.png)

### unknown 타입

- 모든 타입의 슈퍼타입
- 업캐스팅은 모두 허용! - unknown에 모든 타입을 할당할 수 있다
- 다운캐스팅은 모두 불가 ! ⇒  unknown은 다른 어떤 타입에도 할당할 수 없다

### never 타입

- 모든 타입의 서브타입
- 모든 타입의 부분집합 - 공집합!
- 업캐스팅은 모두 허용 ! - never를 모든 타입에 할당 가능
- 다운 캐스팅은 모두 불가! ⇒ 어떤 타입도 never에 할당 할 수 없다
- 어떤 값도 저장 돼서는 안되는 변수의 타입으로 사용하기 좋다

### void 타입

- undefined 타입의 슈퍼타입

```tsx
let voidVar : void = undefined; // 업캐스팅이므로 허용

//반환값이 void인 함수에 undefined 반환도 가능
function voidExam(){
	function voidFunc() : void{
		console.log("hi");
		return undefined;
		}
}
```

### any 타입 - 치트키

: 타입 계층도를 완전히 무시 → 웬만하면 쓰지 말자

- 모든 타입의 superkey로도, 모든 타입의 subkey로도 적용됨(never 제외)

```tsx
let unknownVar:unknown;
let anyVar : any;
let undefinedVar : undefined;
let neverVar : never;
//any 타입에 unknown 할당 가능 (다운캐스팅 가능)
anyVar = unknownVar;
//undefined 타입에 any타입 할당 가능(다운캐스팅 가능)
undefinedVar = anyVar;

//-- error ! never type에는 아무것도 다운캐스팅 불가
neverVar = anyVar ; 
```

## 객체 타입의 호환성

: 어떤 객체 타입을 다른 객체타입으로 취급해도 괜찮은가?

- **프로퍼티를 기준**으로 부모-자식 관계를 형성
- 기본타입과 마찬가지로 업캐스팅은 가능, 다운캐스팅은 불가능

```tsx
//슈퍼타입
type Book = {
	name: string;
	price: string;
}

type ProgrammingBook = {
	name: string;
	price: string;
	skill : string;
}

let book: Book;
let programmingBook : ProgrammingBook = {
	name : "한입",
	price : 33000,
	skill : "reactjs"
};
book = programmingBook; //upcasting - 가능!

//초과 프로퍼티 검사

let book2 : Book = {
	name : "한입",
	price : 33000,
	skill : "reactjs" // --error!
};

let book3: Book = programmingBook; //리터럴 x - 허용
```

### 초과 프로퍼티 검사

: 객체 초기화시 리터럴로 선언하는 경우, 초과 프로퍼티를 작성하면 안되도록 막는 검사

 - 함수 인수로 객체 전달시에도 객체 리터럴 말고 변수로 저장해뒀다가 그 변수를 전달해야 오류가 발생하지 않는다

즉, 객체 리터럴을 사용하면 정의해놓은 프로퍼티만! 사용 가

## 대수 타입

: 여러 개의 타입을 합성해서 새롭게 만들어낸 타입

### 1. 합집합 타입 - union type

```tsx
// string number union type
let a: number |string;
a = 1;
a = 'hello';

let arr:(number|string|boolean)[] =[1,"hello",true];

//객체 유니온
type Dog = {
	name : string;
	color : string;
}
type Person = {
	name : string;
	language : string;
}
type Union1 = Dog | Person

let union1 : Union1 = {
	name :"",
	color : ""
};
let union2 : Union1 = {
	name :"",
	language : ""
};
let union3 : Union1 = {
	name :"",
	color: "",
	language : ""
};

let union4: Union1 = {
	name :""
}; // --error !!
```

- 필요한 만큼 union 가능
- 객체의 union : 한 개의 집합에만 속하거나, 교집합이어야만 유니언 타입에 포함된다!

### 2. 교집합 타입 - intersection

```jsx
let variable : number & string; //variable - never 타입

let Intersection = Dog & Person;

let intersection : Intersection = {
	name: "",
	color:"",
	
```

- 기본 타입에서 사용하면 교집합 타입은 웬만하면 never
- 객체에서 많이 사용함!
    - 교집합에 해당하는 객체가 해당됨 → 모든 프로퍼티를 가지고 있는 객체만  교집합 타입에 포함된다

## 타입 추론

### 타입스크립트는 어떤 상황에 타입을 추론하는가?

1. 일반적인 변수 선언 상황
    - 기준 : 변수의 초기 값
2. 함수의 반환값 - return 문 다음 변수 기준

⇒ 추론이 가능한 상황에 추론한다고 생각하면 된다

### any 타입의 진화

변수 선언 시 초기값을 지정하지 않으면 **암묵적인 any 타입**으로 추론됨

→ 이 경우, 이 변수에 **할당되는 값에 따라서 타입이 진화됨**

명시적으로 any타입으로 선언하는 것과 다르게 동작!  - 게속 any타입임

### const키워드를 통한 변수 선언

할당한 값의 리터럴 타입으로 추론됨!

### 타입 넓히기

: 상수 추론처럼 리터럴로 추론하는게 아니라, 더 범용적으로 이 변수를 사용할 수 있도록 더 넓은 타입으로 추론해주는 과정

## 타입 단언 type assertion

어떤 상황에 필요한가?

```jsx
type person = {
	name : string;
	age : string;
};
let person = {}; // 이 빈 객체를 기준으로 타입이 추론되어버림
person.name = "이정환"; // --error!! : 
person.age = 27;

//=> 타입 단언
let person = {} as person;

//객체 리터럴로 정의할 때, 초과 프로퍼티를 넣고 싶을 때도 사용 가능

```

### 타입 단언의 규칙

단언식 : 값 as 단언 ( A as B)

- A가 B의 슈퍼타입이거나
- A가 B의 서브타입이어야 함

다중단언 시, 규칙에 맞지 않는 단언도 가능하지만, 웬만하면 쓰지 말자!

### const 단언

```jsx
let num4 = 10 as const;
// num4의 타입은 리터럴 10
```

**객체 타입과 함께 사용할 때 효과 있음!**

```jsx
let cat = {
	name : "야옹이",
	color : "yellow"
} as const;

cat.name = ''; //error!!
```

→ 모든 프로퍼티가 read-only가 됨

### Non Null단언

: 어떤 값이 null이거나 undefined가 아니라고 타입스크립트 컴파일러에게 알려주는 역할

```jsx
type Post = {
	title : string;
	author?: string;
};

let post:Post ={
	title : "post1",
	author : "author1",
};

const len: number = post.author?.length;
//옵셔널 체이닝 : post.author 값이 없으면 len이 undefined가 되게 하는 연산자임
// 값 자체가 undefined가 될 수 있기 때문에 에러가 발생
// 

// -> 
const len : number = post.author!.length;
// 컴파일러에게 이건 Null이 아닐거라고 믿게 만듦
```

**단언은 실제로 타입을 바꾸는 것과는 다름! - 단언은 조심하자~**

## 타입 좁히기

: 조건문 등을 이용해 넓은 타입에서 좁은 타입으로 타입을 상항에 따라 좁히는 방법을 뜻함

### 타입 가드

: 타입 좁히기를 수행하는 조건문

- typeof
- instanceof 클래스
- in 객체

```jsx
type Person = {
  name: string;
  age: number;
};

function func(value: number | string | Date | null | Person) {
  if (typeof value === "number") {
    console.log(value.toFixed());
  } else if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (value instanceof Date) {
    console.log(value.getTime());
  } else if (value && "age" in value) {
    console.log(`${value.name}은 ${value.age}살 입니다`)
  }
}
```

## 서로소 유니온 타입 / tagged union type

: 교집합이 없는 타입들로만 만든 유니온 타입.

- 타입 좁히기가 가능하지만, 더욱 직관적인 타입 확인을 위해 태그 프로퍼티를 사용해서 서로소 유니온 타입으로 만들어줌

```jsx
type Admin = {
  tag: "ADMIN";
  name: string;
  kickCount: number;
};

type Member = {
  tag: "MEMBER";
  name: string;
  point: number;
};

type Guest = {
  tag: "GUEST";
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

function login(user: User) {
  switch (user.tag) {
    case "ADMIN": {
      console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
      break;
    }
    case "MEMBER": {
      console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
      break;
    }
    case "GUEST": {
      console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
      break;
    }
  }
}
```

- 동시에 여러가지 상태를 표현해야 되는 객체 타입을 정의할 때는, 선택적 프로퍼티를 사용하는 것보다, 상태에 따라서 타입들을 분리해서 태그 같은 프로퍼티를 추가해서 서로소 유니온 타입으로 만들어주는게 switch case문같은 걸 사용해서 더욱 직관적인 코드 작성 가능

```jsx

```