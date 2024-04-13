# Item 7. 타입이 값들의 집합이라고 생각하기

***타입 연산자는 인터페이스의 속성이 아닌, 값의 집합 (타입의 범위)에 적용된다.***

strictNullChecks 설정에 따라 null, undefined는 다른 타입의 종속되거나 안 될 수 있음

never 는 가장 작은 타입(집합)으로 아무 값도 포함하지 않음. 어떤 값도 할당할 수 없다.

```tsx
const x : never = '1' // error 
```

유닛 (unit), 리터럴 (literal): 한 가지 값만 포함하는 타입

```tsx
type A = 'A';
type B = 'B';
```

유니온 (union): 두 개 혹은 세 개로 묶은 타입, 값 집합들의 합집합

```tsx
type ab = 'a' | 'b';
```

인터섹션 (intersection): 값 집합들의 교집합

```tsx
interface Person {
  name:string;
}
interface Lifespan {
  birth:Date;
  death?: Date;
}
type personSpan = Person & Lifespan

// intersection (교집합)이므로 PersonSpan을 공집합일 것으로 예상하지만
const AT: personSpan = {
  name: 'Alan Turing';
  birth: new Date('1912/06/23');
  death: new Date('1954/06/07');
} //정상으로 처리 됨.
```

⇒ 구조적 타이핑 때문! 

아래의 경우로 명확하게 설명할 수 있음

```tsx
type K = keyof ( Person | Lifespan ); //타입이 never, Person | Lifespan 에 해당하는 값은 어떠한 속성도 없기 때문에

keyof (A&B) = (keyof A) | (keyof B)
keyof (A|B) = (keyof A) & (keyof B)
```

⇒ 마찬가지로 덕 타이핑, 구조적 타이핑 때문!

좀 더 일반적인 경우, extends 키워드를 사용해서 상속의 관계로 표현

```tsx
interface Person {
  name: string;
}

interface PersonSpan extends Person{
  birth : Date;
  death? : Date;
}
```

그러나, 엄격한 상속의 관계가 아닐 경우 집합의 관점으로 표현하는 것을 권장함.

```tsx
interface Person {
  name: string;
}

interface PersonSpan {
	name: string;
  birth : Date;
  death? : Date;
}
```

배열과 튜플의 관계 역시 명확해짐

```tsx
const triple : [number,number,number] = [1,2,3];
const double: [number,number] = triple; //error
//Type '[number, number, number]' is not assignable to type '[number, number]'.
//Source has 3 element(s) but target allows only 2
```

차이점은 원소의 길이와 특정 인덱스에 맞는 타입을 지정할 수 있다는 것.