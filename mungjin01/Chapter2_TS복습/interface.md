# 인터페이스

# 인터페이스

### 인터페이스란?

타입스크립트에서 인터페이스는 객체 타입을 정의할 때 사용하는 문법으로 인터페이스로 타입을 정의할 수 있는 부분은 다음과 같다

예시를 보면

```tsx
let person = { name: "Capt", age: 28 };

function logAge(obj: { age: number }) {
  console.log(obj.age); // 28
}
logAge(person); // 28
```

여기서 logAge() 함수에서 받는 인자의 형태는 age를 속성으로 갖는 객체이다. 이렇게 인자를 받을 때 단순한 타입뿐만 아니라 객체의 속성 타입까지 정의할 수 있는데

만약 여기에 인터페이스를 적용한다면?

```tsx
interface personAge {
  age: number;
}

function logAge(obj: personAge) {
  console.log(obj.age);
}
let person = { name: "Capt", age: 28 };
logAge(person);
```

logAge()의 인자는 personAge라는 타입을 가져야한다.

### 인터페이스의 옵션 속성

인터페이스로 정의된 객체의 속성을 선택적으로 사용하고 싶을 때 옵션 속성을 사용한다

```tsx
interface 인터페이스_이름 {
  속성?: 타입;
}
```

속성의 끝에 ? 붙이기

```tsx
interface CraftBeer {
  name: string;
  hop?: number;
}

let myBeer = {
  name: "Saporo",
};
function brewBeer(beer: CraftBeer) {
  console.log(beer.name); // Saporo
}
brewBeer(myBeer);
```

### 인터페이스의 상속

클래스를 상속 받을 때 extends라는 예약어를 사용한다. 인터페이스를 상속받을 때도 동일하게 extends 예약어를 사용한다

```tsx
interface Person {
  name: string;
  age: number;
}
```

이렇게 Person 인터페이스에 name과 age 속성이 있고

```tsx
interface Developer extends Person {
  skill: string;
}
```

이후 Developer 인터페이스는 Person 인터페이스를 상속받아 속성을 추가했다 이렇게 상속받으면 Developer 인터페이스는 아래처럼 정의한 것과 같아서

```tsx
interface Developer extends Person {
  name: string;
  age: number;
  skill: string;
}
```

Developer 인터페이스를 지정한 객체는 Person과 Developer의 속성들을 모두 사용할 수 있다

```tsx
let webDeveloper: Developer = {
  name: "bumgu",
  age: 18,
  skill: "typescript",
};
```

그런데 상속을 할 때, 그러니까 상위 인터페이스의 타입을 하위 인터페이스가 상속받아 타입을 정의할 때 상위 인터페이스의 타입과 호환이 되어야 한다. (= 상위에서 정의된 타입을 사용해야 한다)

```tsx
interface Person {
  name: string;
  age: number;
}

interface Developer extends Person {
  name: number;
}
```

여기 위 코드는 Developer 인터페이스가 Person을 상속받지만 이미 정의된 name의 타입을 잘못 사용하고 있다. 이렇게 Developer 인터페이스가 Person 인터페이스를 잘못 확장하고 있고, name이 호환되지 않는다는 에러가 발생하는 것처럼

인터페이스를 상속할 때는 상위 인터페이스에 정의된 타입을 자식 인터페이스에서 보장해주어야 한

### 인터페이스를 이용한 인덱싱 타입 정의

인덱싱이란 객체의 특정 속성을 접근하거나 배열의 인덱스로 특정 요소에 접근하는 것을 말한다

(ex console.log(객체[’user’]); or console.log(배열[0]);)

```tsx
interface SalaryInfo {
  junior: string;
  senior: string;
}
```

여기 SalaryInfo 인터페이스에 주니어와 시니어에 대한 타입이 정의 되어있다고 할 때

```tsx
let salary: SalaryInfo = {
  junior: "100원",
  senior: "200원",
};
```

SalaryInfo 인터페이스에는 속성이 junior와 senior 두 가지 속성밖에 없어서 다른 속성을 선언할 수 없다

예를 들어

```tsx
let salary: SalaryInfo = {
  junior: "100원",
  senior: "200원",
  ceo: "500원",
};
```

이렇게 선언을 하면 당연히 에러가 발생한다 근데 만약 salary에 고정된 junior, senior외에 다른 속성들이 들어온다면 계속해서 인터페이스를 수정해줘야한다는 번거로움이 생긴다

이때 사용하는 것이 인덱스 시그니처이다

인덱스 시그니처는 정확한 속성 이름을 명시하지 않고 속성 이름의 타입과 속성 값의 타입을 정의하는 문법을 말한다

위의 인터페이스를 인덱스 시그니처를 사용한 인터페이스로 바꿔보면

```tsx
interface SalaryInfo {
  [positon: string]: string;
}
```

속성의 이름은 string 타입이고, 속성 값 타입은 string이라는 의미이며, 속성의 이름값은 string 타입의 어떤 것이든 상관없다

```tsx
let salary: SalaryInfo = {
  junior: "100원",
  mid: "400원",
  senior: "700원",
  ceo: "0원",
  newbie: "50원",
};
```

---

레퍼런스

- 쉽게 시작하는 타입스크립트
