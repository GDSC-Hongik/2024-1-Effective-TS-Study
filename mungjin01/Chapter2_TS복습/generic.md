# 제네릭

제네릭은 타입을 미리 정의하지 않고 사용하는 시점에 원하는 타입을 정의해서 쓸 수 있는 문법으로 함수의 파라미터와 같은 역할을 한다.

여기에서 역할이란 함수의 인자에 넘긴 값을 함수의 파라미터로 받아 함수 내부에서 그대로 사용하는 방식으로 예시를 보면

```tsx
function getText(text) {
  return text;
}
```

여기서 getText()는 text를 받아서 그대로 반환해준다. 문자열이 들어오거나, 정수가 들어와도 그대로 반환을 하는데 이 원리를 타입스크립트에 대입해 ‘타입을 넘기고 그 타입을 그대로 반환받는다’가 제네릭의 원리이다

### 제네릭 기본 문법

```tsx
function getText<T>(text: T): T {
  return text;
}
```

getText<string>('hi') 와 같이 호출을 할 수 있는데 그럼

```tsx
function getText(text: string): string {
  return text;
}
```

라고 선언한 것과 같고 getText<number>(10) 이렇게 호출하면 타입이 number로 지정된 것과 같다

### 왜 제네릭을 사용할까?

타입을 미리 정의하지 않고 호출할 때 타입을 정의해서 사용할까?

→ 반복되는 타입 코드를 줄여주기 때문

```tsx
function getText(text: string): string {
  return text;
}
function getNumber(num: number): number {
  return num;
}
```

첫 번째 getText() 함수는 문자열 텍스트를 받아 그대로 반환해주기 때문에 파라미터 타입과 반환 타입을 모두 string으로 선언했다. getNumber() 함수는 숫자 텍스트를 받아 그대로 반환해주니까 파라미터 타입과 반환 타입을 모두 number로 선언했다

이 두 함수 모두 텍스트를 넘겨받아 그대로 반환해 주는 코드에 타입만 다르게 선언한 것이다

이어서 여기에 다른 데이터 타입들이 추가된다면?

```tsx
function getBool(bool: boolean): boolean {
  return bool;
}
function getArr(arr: []): [] {
  return arr;
}
function getObj(obj: {}): {} {
  return obj;
}
```

파라미터와 반환값의 타입만 다르지 인자로 받아온 값을 그대로 넘겨준다는 점에서 똑같은 코드이다 DRY(Don’t repeat yourself)라는 개발 원칙에 어긋난다

이때 제네릭을 사용하면

```tsx
function getSomething<T>(something: T): T {
  return something;
}
```

반복되는 5개 함수가 하나로 줄어든다 깔끔 ^\_^

그러면 any를 사용해도 되지않을까라는 생각이 들수도 있는데

any를 사용해도 되지만 any는 자바스크립트처럼 모든 타입을 취급하는 대신 타입스크립트의 코드 가이드나 에러방지 혜택을 받지 못해서 좋은 방법은 아니라고 한다

### 인터페이스에 제네릭 사용하기

제네릭은 함수뿐만 아니라 인터페이스에도 사용할 수 있다

```tsx
interface ProductDropdown {
  value: string;
  selected: boolean;
}

interface StockDropdown {
  value: number;
  selected: boolean;
}
```

여기서 value에 다른 데이터 타입을 갖는 드랍다운이 필요하면 어떻게 해야할까?

```tsx
interface Dropdown<T> {
  value: T;
  selected: boolean;
}
```

인터페이스 속성 중 제네릭으로 받은 타입을 사용할 곳에 T타입을 연결해준다

### 제네릭의 타입 제약

모든 타입이 아니라 몇 개의 타입만 제네릭으로 받고 싶다면 어떻게 해야 할까?

```tsx
function embraceEverything<T extends string>(thing: T): T {
  return thing;
}
```

제약할 타입은 string이기 때문에 <T extends string> 이렇게 정의하면 이 함수를 호출할 때 다음과 같이 제네릭에 string 타입을 넘길 수 있다

```tsx
embraceEverything<string>("hi");
```

이 코드는 타입 에러 없이 잘 실행된다 그치만 string이 아닌 다른 타입을 제네릭으로 넘기려고 하면 에러가 발생한다

ex)

```tsx
embraceEverything<number>("hi");
```

이러면 ‘number’ 형식이 ‘string’ 제약 조건을 만족하지 않습니다라고 에러가 뜬다

embraceEverything() 함수에 넘길 수 있는 타입은 string 타입뿐인데 다른 타입을 썼으니 당연히 에러가 발생한다. 이렇게 extends 키워드를 사용해서 제네릭의 타입을 제약할 수 있다.

---

레퍼런스

- 쉽게 시작하는 타입스크립트
