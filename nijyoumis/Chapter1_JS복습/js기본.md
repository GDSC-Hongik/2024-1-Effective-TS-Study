# Section01. javascript 기본

# javascript

## HTML의 역할

- 요소들의 내용, 배치, 모양을 정하기 위해 사용하는 언어
- 색상이나 디자인 등의 수정은 불가.

## CSS의 역할

- 요소들의 색상, 크기 등의 스타일을 설정

## Javascript의 역할

- 웹 내부에서 발생하는 다양한 기능을 만들 수 있는 언어
- 웹의 근육!

## Javascript의 실행

- 엔진에 의해 실행
- 엔진은 웹 브라우저에 설치되어있음

# 문법

## 변수와 상수

### 1. 변수

선언 시 초기화하지 않아도 됨, 초기화 하지 않으면 → undefined

- let : 이름으로 구별 → 중복되는 이름으로 다시 선언할 수 없음

```jsx
let age = 27;
console.log(age);
```

### 2. 상수

한 번 저장된 값을 다시 바꿀 수 없음

→ 선언 시 꼭 초기화 해야함!

```jsx
const birth = "1997.01.07";
birth = "123"; //오류!!
```

### 3. 변수 명명 규칙(네이밍 규칙)

1. $, _ 제외한 기호 사용 불가능
2. 숫자로 시작할 수 없다
3. 예약어를 사용할 수 없다 ( let, if, ..)
4. 의미 있는 변수명 사용하기

## 자료형(타입)

Type = 집합 

: 동일한 속성이나 특징을 가진 원소들을 묶은 것

- 원시타입 : 기본형 타입
    - Number : 모든 숫자형, Infinity, -Infinity, NaN
    - String
    - Boolean
    - Null
    - Undefined
- 객체타입
    - Object
        - Array
        - Funcion
        - RegexExp

### 1. Number

: 모든 숫자형, Infinity, -Infinity, NaN

- NaN 이 존재 → 잘못된 연산 해도 NaN이 떠서 프로그램이 완전 종료되지는 않는다

### 2. String

```jsx
//concatenate
let myName = "이정환";
let myLocation = "목동";
let introduce = myName + myLocation ; // 이정환 목동
```

**템플릿 리터럴 문법**

- 백틱 `` -> ${변수의 값}을 동적으로 문자열에 포함시킬 수 있다

let introduceText = `${myName}은 ${myLocation}에 거주합니다`;

### 3. Boolean

: TRUE / FALSE

### 4. Null

: 아무것도 없다

### 5. undefined

: 초기화 값이 없을 때, 자동으로 할당되는 값

- null과의 다른점 !!
    
    : null은 직접 명시적으로 할당해줘야하는 값임.
    

## 형 변환 Type Casting

: 어떤 값의 타입을 다른 타입으로 변경하는 것

### 1. 묵시적 형 변환

: 하나의 변수의 값을 형 변환 했을 때, 오류가 발생하지 않고 연산이 종료되는 경우에만 일어남

num+str → num +num

### 2. 명시적 형 변환

: 내장함수 등을 이용해 직접 형 변환을 명시

1. 문자열 → 숫자
    - `Number()`
    - `ParseInt()` : 숫자만 들어있지 않은 변수에서 숫자로 형 변환 하고싶을 때 사용 ( “10개”)
        - “asdf10” → parseInt로도 형 변환 불가능!
2. 숫자 → 문자열
    - `String()`

## 연산자

### 1. 대입 연산자: =

### 2. 산술 연산자 : +,-, *, / , %

### 3. 복합 대입 연산자 : +=, -=, *=, /=, %=

### 4. 증감 연산자 : ++, —

- 전위 연산 : 변수 앞 → 해당 line부터 적용
- 후위 연산 : 변수 뒤 → 다음 line부터 적용

### 5. 논리 연산자: OR, AND, NOT

|| , && !

### 6. 비교 연산자

- == : 값만 비교
- === : 값과 자료형 모두 비교!
    
    → 세 개로 쓰는게 권장됨.
    
- 대소비교 : >, < , < =, > =

### 7. null 병합 연산자

: 피연산자 중 **존재하는 값**을 추려내는 기능 

```jsx
let var; //undefined
let var2 = 10;
let var3 = 20;

let var4 = var1 ?? var2;
console.log(var4); // 10
```

- 두 피연산자 모두 null이나 undefined가 아닌 경우 → 제일 앞에 있는 피연산자 반환
    
    ```jsx
    let var5 = var2 ?? var3 ;
    console.log(var5); // 10 (var2)
    ```
    

### 8. typeof 연산자

: 값의 타입을 **문자열로** 반환

### 9. 삼항 연산자

: 조건식을 이용해서 참, 거짓일때의 값을 다르게 반환

## 조건문

### 1. if 문

: 복잡한 여러 개의 조건을 이용할 때

반드시 if ~ else 혹은 if~else if 로 끝나야함!

### 2. switch 문

: 변수의 값을 기준으로 다른 코드 수행할 때

다수의 조건을 처리할 때 if 보다 직관적임

- 모든 case 확인 → 일치하는 코드의 아래에 있는 모든 코드를 다 실행함

```jsx
let animal = "cat";
switch(animal){
	case "cat":{
		console.log("고양이");
		break;
	}
		case "dog":{
		console.log("개");
		break;
	}
		case "bear":{
		console.log("곰");
		break;
	}
	default:{
		console.log("그런 동물은 전 모릅니다..");
	}
}
```

## 반복문

### for문

```jsx
for(초기식; 조건식; 증감식){
	반복문;
}
```

- 조건식을 건들지 않고 반복문을 빠져나오고 싶을 때

→ for문 안에 if문과 break문 사용

- 특정 회차만 건너 뛰고 싶을 때

→ if문과 continue문 사용

## 함수

### 함수 선언

function fName (){} 

- 함수 호출 시, 반드시 소괄호와 함께 작성

### 호이스팅

선언문을 내부적으로 먼저 실행

## 함수 표현식과 화살표 함수

```jsx
function funcA(){
	console.log("funcA");
}
let varA = funcA; // 소괄호 없이 할당
varA(); // funcA
```

- 선언식 : 어떤 변수의 값으로 담기지 않은 상태로 유지가 되어야 하는 것.

함수는 값으로서 취급되는데, 취급만 값으로 되는거고, 어떤 변수에 담기지 않은 상태를 선언식, 어떤 변수에 담긴! 상태가 함수 표현식인거지!

```jsx
let varB = function funcB(){
		console.log("funcB");
		} // -> varB에 담겨있음 -> 선언식 x!!
		
varB(); // error!!
```

### 함수 표현식

```jsx
let varB = function (){
		console.log("funcB");
		} 
varB(); 
```

함수 표현식으로 만든 함수들 → **값**으로써 취급 ⇒ **호이스팅의 대상이 되지 않음!**

### 화살표 함수

```jsx
let varC = () =>{};
```

## 콜백함수

: 자신이 아닌 다른 함수에, 인수로써 전달된 함수

- 다른 함수에 전달해서 나중에 호출시키도록 설정한 함수

```jsx
function main(value){
	console.log(value);
}
function sub(){
	console.log("i am sub");
}
main(sub); // sub함수 자체가 출력됨 : sub(){console.log("i am sub");
}
//sub함수를 main에서 호출하고 싶으면
function main(value){
	value();
} // i am sub
```

- Callback : 뒷전에 실행되는, 나중에 실행되는 ..

### 콜백함수의 활용

구조가 비슷한 함수 - 매번 새로 생성 x

```jsx
function repeat(count, callback){
	for (let idx = 1; idx <= count; idx++){
		callback(idx);
		}
	}
repeat(5, (idx)=>{console.log(idx);});
repeat(5, (idx)=>{console.log(idx*2);});
repeat(5, (idx)=>{console.log(idx*3);});
```

## 스코프

### 1. 전역 스코프

: 전체 영역에서 접근 가능

### 2. 지역 스코프

: 특정 영역에서만 접근 가능

- 조건문, 함수 등 **블록 내에서 선언한 변수**
- 함수 선언식은 함수 내에서만 지역 스코프를 가짐
    - 반복문 안에서 선언한 함수는 전역 스코프

## 객체

- 원시 타입이 아닌 객체 타입의 자료형

### 객체 생성

1. 객체 생성자

```jsx
let obj1 = new Object();
```

1. 객체 리터럴(대부분 사용)

```jsx
let obj2 = {};
```

### 객체 프로퍼티 (객체 속성)

key: value, 

- key : 문자열, 숫자
    - 문자열 - 따옴표 안감싸줘도 됨
- 접근
    - 점 표기법 (person.name)
        - 존재하지 않는 값에 접근 → undefined
    - 괄호 표기법 (person[”age”])
        - **문자열**로 프로퍼티의 키를 시
        - 따옴표로 감싸주지 않으면 변수로 인식해서 오류 발생
- 새로운 프로퍼티 추가, 수정

```jsx
person.job = "fe developer";
person["favoriteFood"] = "떡볶이";
```

- 프로퍼티 삭제 - `delete` 메서드 사용

```jsx
delete person.job;
delete person["job"];
```

- 프로퍼티의 존재 유무를 확인하는 법 - `in` 메서드 용

```jsx
let result1 = "name" in person;
```

### 상수 객체

: 상수에 저장해 놓은 객체

- 재정의하면 오류가 발생한다는 상수의 특성은 같음
- 프로퍼티를 추가, 수정, 삭제는 문제가 되지 않는다

### 메서드

: 객체 프로퍼티 중 값이 함수인 프로퍼티

```jsx
const person = {
	name = "이정환",
	sayHi(){
		console.log("안녕!");
		},
	};
person.sayHi();
person.["sayHi"]();
```

- 메서드는 객체의 동작을 정의하는데 주로 사용

## 배열

: 여러개의 값을 **순차적**으로 담을 수 있는 자료형

- 어떤 타입의 값이든 담을 수 있음!

```jsx
let arr = [1,2,3,true,"hello",null,undefiend,()=>{}, {},[]];
```

### 배열 요소 접근

```jsx
let item1 = arr[0];
arr[0] = "hello";
```