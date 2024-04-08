# section 4. 함수와 타입

## 함수 타입

### 함수를 설명하는 가장 좋은 방법

: 어떤 타입의 매개변수를 받고, 어떤 타입의 결과값을 반환하는지 이야기 해주는 것

```jsx
function func(a: number, b: number):number{
	return a*b;
}
```

### 화살표 함수의 타입을 정의하는 방법

```jsx
const add = (a:number , b: number): number=> a+b;
```

### 함수의 매개변수

```jsx
function introduce(name="이정환"){
	console.log(`name : ${name});
}
```

1. 기본값과 다른 타입으로 매개변수의 타입을 정의하면 에러
2. 함수 호출 시 자동 추론된 매개변수의 타입과 다른 인수 전달 시 에러

### 선택적 매개변수

```jsx
function introduce(name = "이정환", tall?: number) {
  console.log(`name : ${name}`);
  console.log(`tall : ${tall}`);
}

introduce("이정환", 156);

introduce("이정환");
```

- tall 의 타입 : number | undefined
- tall 에 연산을 하고 싶어도, undefined 일 수 있으므로 타입스크립트가 막음 → 타입 좁히기 후에 연산해야!
- 선택적 매개변수는 반드시 필수 매개변수 이후에 선언돼야 함

### rest parameter

: 가변적인 길이의 인수를 전달하면 배열로 묶어서 rest라는 변수에 저장하는 문법

```jsx
function getSum(...rest: number[]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}
```

- 길이를 고정하고 싶다면 - 튜플 타입 이용

```jsx
function getSum(...rest: [number, number, number]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}

getSum(1, 2, 3)    // ✅
getSum(1, 2, 3, 4) // ❌
```

## 함수 타입 표현식과 호출 시그니처

### 함수 타입 표현식

```jsx
type Operation = (a: number , b: number) => number;

const add: Operation = (a,b) =>a+b;
const sub: Operation = ...
const multiply : ...
```

- 함수의 타입을 표현식으로 정의할 때는 매개변수의 개수와 타입을 다 맞춰줘야 함!

```jsx
const add: (a: number, b: number) => number = (a, b) => a + b;
```

### 호출 시그니처 call signature

: 객체를 정의하듯 함수 타입 정의

- js 에서는 함수도 객체이기 때문에 가능

```jsx
type Operation2 = {
  (a: number, b: number): number;
};

const add2: Operation2 = (a, b) => a + b;
const sub2: Operation2 = (a, b) => a - b;
const multiply2: Operation2 = (a, b) => a * b;
const divide2: Operation2 = (a, b) => a / b;

```

### 하이브리드 타입

: 객체로도, 함수로도 사용할 수 있는 타입

- 거의 사용 안함!

```jsx
type Operation2 = {
  (a: number, b: number): number;
  name: string;
};

const add2: Operation2 = (a, b) => a + b;
(...)

add2(1, 2);
add.name;
```

## 함수 타입의 호환성

: 특정 함수 타입을 다른 함수 타입으로 취급해도 괜찮은지를 판단하는 것

### 1. 반환값의 타입이 호환되는가

```jsx
type A = () => number;
type B = () => 10;

let a: A = () => 10;
let b: B = () => 10;

a = b; // ✅
b = a; // ❌
```

### 2. 매개변수의 타입이 호환되는가

1. 매개변수의 개수가 같을 때
    
    매개변수의 **타입**을 기준으로 함수 타입의 호환성을 판단할 때
    
    - 업캐스팅 안됨! (슈퍼타입 ← 서브타입)(x)
    - 다운캐스팅 됨! (서브타입 ← 슈퍼타입)(o)
    
    ```jsx
    type C = (value: number) => void;
    type D = (value: 10) => void;
    
    let c: C = (value) => {};
    let d: D = (value) => {};
    
    c = d; // ❌
    d = c; // ✅
    ```
    
    **매개변수가 객체 타입일 때**
    
    ```jsx
    type Animal = {
      name: string;
    };
    
    type Dog = {
      name: string;
      color: string;
    };
    
    let animalFunc = (animal: Animal) => {
      console.log(animal.name);
    };
    
    let dogFunc = (dog: Dog) => {
      console.log(dog.name);
      console.log(dog.color);
    };
    
    animalFunc = dogFunc; // ❌
    
    let animalFunc = (animal: Animal) => {
      console.log(animal.name);  // ✅
      console.log(animal.color); // ❌
    };
    
    dogFunc = animalFunc; // ✅
    
    let dogFunc = (dog: Dog) => {
      console.log(dog.name);
    };
    ```
    
    왜? 서브타입이 더 구체적 ..
    
2. 매개변수의 개수가 다를 때
    - 할당하려고 하는 함수의 타입의 매개변수의 개수가 더 적을 때에만  호환 가능 (타입이 같은 경우)
    
    ```jsx
    type Func1 = (a: number, b: number) => void;
    type Func2 = (a: number) => void;
    
    let func1: Func1 = (a, b) => {};
    let func2: Func2 = (a) => {};
    
    func1 = func2; // ✅
    func2 = func1; // ❌
    ```
    

## 함수 오버로딩

: 함수를 매개변수의 개수나 타입에 따라 여러가지 버전으로 정의하는 방법

### 오버로드 시그니처

: 구현부 없이 선언식만 써 놓은 것

: 오버로딩을 위해 매개변수 별로 다른 버전을 명시하기 위해 작성

- 정의 시, 실제 구현부가 아닌 오버로드 시그니처에 정의된 매개변수의 개수나 타입에 따름.
- 오버로드 시그니처의 매개변수 개수에 차이가 있다면, 실제 구현부에서 ?(옵셔널 체이닝) 사용해서 선언해줘야 함

### 구현 시그니처

: 실제 구현부

```jsx
/**
 * 함수 오버로딩
 * 같은 함수를 매개변수의 개수나 타입에 따라
 * 여러가지 버전으로 만드는 문법
 * -> 하나의 함수 func
 * -> 일단 모든 매개변수는 넘버타입
 * -> Ver1. 매개변수가 1개일 때에는 매개변수에 20을 곱한 값을 출력
 * -> Ver2. 매개변수가 3개일 때에는 모든 매개변수를 더한 값을 출력
 */
 
 // 버전들 -> 오버로드 시그니쳐
function func(a: number): void;
function func(a: number, b: number, c: number): void;

// 실제 구현부 -> 구현 시그니쳐
function func(a: number, b?: number, c?: number) {
  if (typeof b === "number" && typeof c === "number") {
    console.log(a + b + c);
  } else {
    console.log(a * 20);
  }
}

func(1);        // ✅ 버전 1 - 오버로드 시그니쳐
func(1, 2);     // ❌ 
func(1, 2, 3);  // ✅ 버전 3 - 오버로드 시그니쳐
```

## 사용자 정의 타입 가드 custom type guard(is)

: 참 또는 거짓을 반환하는 함수를 이용해 타입 가드를 만들 수 있도록 하는 것

- 함수 반환값으로 타입이 좁혀지지 않음

```jsx
function isDog(animal : Animal){
	return (animal as Dog).isBark !== undefined;
}
function warning(animal : Animal){
	if(isDog(animal)){
	animal; // 여전히 type : Animal
	}else if ("isScratch" in animal){
	}
};
```

- in 사용도 좋지 않음

```jsx
type Dog = {
  name: string;
  isBark: boolean;
};

type Cat = {
  name: string;
  isScratch: boolean;
};

type Animal = Dog | Cat;

function warning(animal: Animal) {
  if ("isBark" in animal) {
    console.log(animal.isBark ? "짖습니다" : "안짖어요");
  } else if ("isScratch" in animal) {
    console.log(animal.isScratch ? "할큅니다" : "안할퀴어요");
  }
}
```

```jsx
(...)

// Dog 타입인지 확인하는 타입 가드
function isDog(animal: Animal): animal is Dog {
  return (animal as Dog).isBark !== undefined;
}

// Cat 타입인지 확인하는 타입가드
function isCat(animal: Animal): animal is Cat {
  return (animal as Cat).isScratch !== undefined;
}

function warning(animal: Animal) {
  if (isDog(animal)) {
    console.log(animal.isBark ? "짖습니다" : "안짖어요");
  } else {
    console.log(animal.isScratch ? "할큅니다" : "안할퀴어요");
  }
}
```

→ 함수 isDog, isCat의 반환문이 true인경우, animal의 타입은 Dog / Cat이다 라고 정의해줌!