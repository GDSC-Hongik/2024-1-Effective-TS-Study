# Section3. 타입스크립트 이해하기

## 타입스크립트 이해하기

문법만 안다고 활용할 수 있는 쉬운 언어는 아님. 

제대로 기능을 활용하기 위해서는 동작 원리에 대해서 이해할 필요가 있음

<aside>
⭐ ***어떤 기준으로 타입을 정의하는가***

***어떤 기준으로 타입 간의 관계를 정의하는가***

***어떤 기준으로 타입의 오류를 검사하는가*** 

</aside>

## 타입은 집합이다

타입 스크립트가 말하는 타입은 동일한 속성을 갖는 여러 값을 모아 놓은 집합

**ex )** number type ⊃ number literal type

![Untitled](Section3%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20e8a707d1c97d4e8e9af8f940cd817515/Untitled.png)

결국, ts의 타입은 값을 포함한 집합이기 때문에 타입끼리 부모 자식 관계를 갖고, 모든 타입의 관계를 타입 계층도로 표현할 수 있음

타입 호환성을 이해할 수 있게 됨

타입 호환성 : 어떤 타입을 다른 타입으로 취급해도 괜찮은지 판단하는 것

![Untitled](Section3%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20e8a707d1c97d4e8e9af8f940cd817515/Untitled%201.png)

```tsx
let num1: number = 10;
let num2: 10 = 10;
num1 = num2; //ok
num2 = num1: //not ok num2의 type이 더 작은 범주라서
```

업 캐스팅 : subtype의 값을 supertype의 값으로 취급하는 것

다운 캐스팅: supertype의 값을 subtype의 값으로 취급하는  것

![Untitled](Section3%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20e8a707d1c97d4e8e9af8f940cd817515/Untitled%202.png)

![Untitled](Section3%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20e8a707d1c97d4e8e9af8f940cd817515/Untitled%203.png)

## 타입 계층도와 함께 기본타입 살펴보기

업캐스트는 모든 상황에 허용, 다운캐스트는 대부분의 상황에 허용되지 않음. 

Unknown 은 전체 집합으로 볼 수 있음

기본 타입 간의 호환성 확인

```jsx
/*
 * Unknown 타입
 */

function unknownExam() {
    let a : unknown = 1;
    let b : unknown = 'hello';
    let c : unknown = true;
    let d : unknown = null;
    let e : unknown = undefined;

    let unknownVar : unknown; 

    let num: number = unknownVar;    //error
    let str: string = unknownVar;    //error
    let bool: boolean = unknownVar;  //error
}

/*
 * Never 타입
 */

function neverExam() {
    function neverFunc(): never  {
        while(true) {}
    }

    let num: number = neverFunc();    //error
    let str: string = neverFunc();    //error
    let bool: boolean = neverFunc();  //error

    let never1: never = 10;           //error 'number'형식
    let never2: never = 'string';     //error 'string'형식
    let never3: never =  true;        //error 'boolean'형식
}

/*
 * Void 타입
 */

function voidExam() {
    function voidFunc(): void {
        console.log('hi');
        return undefined;
    }  
    // 타입 정의는 void로 했지만, 반환 자체는 undefined로 했기 때문에
    // 아래의 voidVar와 같은 상황
    let voidVar: void = undefined;
}

/*
 * any 타입
 */
function anyExam(){
    let unknownVar: unknown;
    let anyVar: any;
    let undefinedVar: undefined;
    let neverVar : never;

    anyVar = unknownVar;  
    // any가 unknown의 서브타입임에도 불구하고 에러 발생 x
    //any 타입은 다운 캐스팅도 허용 됨.
    undefinedVar = anyVar; 
    // undefined가 any의 서브타입임에도 불구하고 에러 발생 x
    // 결론적으로 any는 다운 캐스팅 하는 것도, 되는 것도 모두 가능

    //예외
    neverVar = anyVar ; // 'never'타입은 순수한 공집합이기 때문에 다운캐스팅될 수 없음
}
```

![Untitled](Section3%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%20e8a707d1c97d4e8e9af8f940cd817515/Untitled%204.png)

## 객체 타입의 호환성

조건이 더 없는 타입, 즉, 프로퍼티가 더 없는 타입이 슈퍼 타입이 됨. 

→ 객체타입의 호환 

```jsx
/*
 * 객체 타입 간의 호환성  
 * -> 어떤 객체 타입을 다른 객체 타입으로 취급해도 괜찮은가?
 */

type Animal = {
    name : string;
    color : string;
};

type Dog = {
    name : string;
    color : string;
    breed : string;
};

let animal : Animal = {
    name :'기린',
    color : 'yellow',
};

let dog : Dog = {
    name : '돌돌이',
    color: 'brown',
    breed: '진도',
};

animal = dog;
dog = animal;// error -> Dog 타입은 Animal 타입의 서브 타입. Animal 타입의 프로퍼티를 다 가지고 있기 때문에

type Book = {
    name : string;
    price : number;
};

type ProgrammingBook = {
    name : string;
    price : number;
    skill : string;
};

let book : Book;
let programmingBook : ProgrammingBook = {
    name : '한 입 크기로 잘라먹는 리액트',
    price : 33000,
    skill : 'reactjs',
};

book = programmingBook;
programmingBook = book;// 'skill' 속성이 'Book' 형식에 없지만, 'ProgrammingBook' 형식에서 필수라서 에러 발생
```

```jsx
/*
 * 초과 프로퍼티 검사 : 변수를 초기화할 때, 객체 리터럴을 초기화 값으로 사용하면 발동. 
 * 실제 타입에 꼭 맞는 프로퍼티만 넣어야 함.
 * 해당 검사를 회피하려면, 미리 선언한 변수를 넣어주어야 함. 함수의 인자로 전달할 떄도 마찬가지
 */
 
type Book = {
    name : string;
    price : number;
};

let book2 : Book = {
    name : '한 입 크기로 잘라먹는 리액트',
    price : 33000,
    skill : 'reactjs',   //에러 -> 초과 프로퍼티 검사라는 ts의 기능 때문
};
```

## 대수 타입

대수 타입 : 여러 개의 타입을 합성해서 새롭게 만들어낸 타입 

- 합집합 타입 - Union 타입

```tsx
let a : string | number | boolean ;

a = 1;
a = 'hello';
a = true;

let arr : (number| string | boolean)[] = [1,'hello',true];

type Dog = {
    name : string;
    color : string;
};

type Person = {
    name : string;
    language : string;
};

type Union1 = Dog | Person

let union1 : Union1 = {
    name : "",
    color : '',
}; //  Dog 타입에 해당

let union2 : Union1 = {
    name : '',
    language :'',
}; //  Person 타입에 해당

let union3 : Union1 = {
    name : '',
    color : '',
    language : '',
}; // Dog와 Person의 교집합에 해당

let union4 : Union1 = {
    name : '',
} // 오류 발생
```

- 교집합 타입 - Intersection 타입

```jsx
let variable : number & string ; // never 타입을 의미

type Dog = {
    name : string;
    color : string;
};

type Person = {
    name : string;
    language : string;
};

type Intersection = Dog & Person;

let intersection1 : Intersection = {
    name : '',
    color : '',
    // language : '', 
}; // language를 주석 처리하면 error 발생
```

## 타입 추론

타입 추론은 일반적으로 초기값, 반환값 등을 기준으로 타입을 추론하는 것.

- 일반적인 케이스

```tsx
let a = 10;
let b = 'hello';
let c = {
    id : 1,
    name : '이정환',
    profile : { 
        nickname : 'winterlood',
    },
    urls : ['https://winterlood.com'],
};

let { id, name, profile } = c;
let [one, two, three] = [1, 'hello', true];

function func(message = 'hello') {
    return 'hello';
}
```

- 예외적인 케이스

```tsx
let d;
d = 10; 
d.toFixed(); //d의 타입은 number

d = "hello";
d.toUpperCase(); //d의 타입은 string
d.toFixed(); // 오류 
```

any의 진화 : 암시적으로 추론된 any 타입은 코드의 흐름에 따라 타입이 변화 

any를 명시적으로 선언하는 것과는 다르게 처리

따라서 암묵적인 any 타입은 사용하지 않는 것을 권장.

- const 의 타입 추론

```tsx
const num = 10;
// 10 Number Literal 타입으로 추론

const str = "hello";
// "hello" String Literal 타입으로 추론
```

상수는 값을 변경할 수 없기 때문에 특별히 가장 좁은 타입으로 추론

- 최적 공통 타입 (Best Common Type)

```tsx
let arr = [1, "string"];
// (string | number)[] 타입으로 추론
```

다양한 타입의 요소를 담은 배열을 변수의 초기값으로 설정하면, 최적의 공통 타입으로 추론됩니다.

## 타입 단언

타입 단언의 형식 : `값 as  타입` , A as B 

타입 단언의 조건 : A가  B의 슈퍼 타입이거나,  서브 타입이어야 함.

```tsx
type Person = {
    name : string;
    age : number;
};

let person : Person = {};  //에러
person.name = '이정환';
person.age = 26; 

let person1 = {} ;
person1.name = '이정환'; //초기값이 빈 배열이고, 해당 값을 기준으로 추론되어서 
person1.age = 26;       //에러 발생

let person1 = {} as Person;  // 타입 단언
person1.name = '이정환';
person1.age = 26; 
```

```tsx
type Dog = {
    name : string;
    color : string;
};

let dog = {
    name : 'dodo',
    color : 'brown',
    breed : 'jindo' //초과 프로퍼티가 존재하지만,  
    //Dog 타입으로 단언하여 초과 프로퍼티 검사를 회피
} as Dog;
```

다중 단언 

```tsx
let num3 = 10 as unknown as string;
```

순서 : number 타입을 unknown 타입으로 단언 → unknown 타입을 string 타입으로 단언

그러나, 권장하지 않음. 단순 눈속임일 뿐이니까 정말 어쩔 수 없는 경우에만 이용하기

const 단언

```tsx
let num4 = 10 as const;
// 10 Number Literal 타입으로 단언됨

let cat = {
  name: "야옹이",
  color: "yellow",
} as const;
// 모든 프로퍼티가 readonly를 갖도록 단언됨
```

타입 단언 때에만 사용할 수 있는 const 타입, 

특정 값을 const 타입으로 단언하면 변수를 const로 선언한 것과 비슷하게 타입 변경

Non Null 단언

```tsx
type Post = {
  title: string;
  author?: string;
};

let post: Post = {
  title: "게시글1",
};

const len: number = post.author!.length;
```

기존의  `값 as 타입` 형식을 따르지 않는 단언 . 단순히 값 뒤에 !를 붙여주면 undefined이거나 null이 아니도록 단언