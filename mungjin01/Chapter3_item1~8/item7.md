# item7 타입이 값들의 집합이라고 생각하기

코드가 실행되기 전, 그러니까 타입스크립트가 오류를 체크하는 순간에 타입을 가지고 있다. 할당 가능한 값들의 집합이 타입이라고 생각하면 된다

가장 작은 집합은 아무 값도 포함하지 않는 공집합으로 ts에서는 never 타입이다

```tsx
const x: never = 12;
// ~ Type '12' is not assignable to type 'never'
```

그 다음으로 작은 집합은 한 가지 값만 포함하는 타입이다 ts에서 유닛 타입이라고 불린다

```tsx
type A = "A";
type B = "B";
type Twelve = 12;
```

두 개 혹은 세 개로 묶으려면 유니온 타입을 사용한다

```tsx
type AB = "A" | "B";
type AB12 = "A" | "B" | 12;
```

타입스크립트로 코드를 짜다보면 꼭 오류에 ‘할당 가능한’ 어쩌구 오류를 볼 수 있는데

```tsx
type AB = "A" | "B";
type AB12 = "A" | "B" | 12;
const a: AB = "A"; // OK, value 'A' is a member of the set {'A', 'B'}
const c: AB = "C";
// ~ Type '"C"' is not assignable to type 'AB'
```

이 문구가 집합의 관점에서 ‘~의 원소’ 또는 ‘~의 부분 집합’을 의미한다

```tsx
interface Person {
  name: string;
}
interface PersonSpan extends Person {
  birth: Date;
  death?: Date;
}
```

타입의 집합이라는 관점에서 extends의 의미는 ‘~에 할당 가능한’과 비슷하게 ‘~의 부분 집합’이라는 의미로 받아들일 수 있다

위의 코드에서 PersonSpan 타입의 모든 값은 문자열 name 속성을 가져야 한다. 그라고 birth 속성을 가져야 제대로 된 부분 집합이 된다

이러한 코드가 있을 때

```tsx
class MyString extends String {
  // ...
}
```

상속의 관점에서 객체 래퍼 타입 String의 서브 클래스를 정의해야겠지만 그렇게 바람직한 방법은 아니라고 한다

그 이유는

1. String 타입은 원시 타입인 문자열을 객체로 감싸기 위해 만들어진 타입이다 따라서 String 타입을 상속한 서브클래스를 사용하면, 객체의 생성과 소멸, 그리고 가비지 컬렉션에 대한 추가적인 비용이 발생할 수 있다
2. String 타입은 불변(immutable) 객체이다 따라서 String 타입을 상속한 서브클래스에서 문자열을 변경하는 것은 적절하지 않는다 이는 의도하지 않은 부작용(side effect)을 발생시킬 수 있다

라고 한다..

너무 어려움..

그래서 해결방안은

'String'타입을 상속한 서브클래스를 만드는 것보다 'String' 타입을 사용하는 것이 더욱 바람직하다고 한다. 예를 들어, 'String' 타입에서 제공하는 메소드를 활용하거나, 필요한 경우에는 문자열을 'String' 타입으로 변환해서 사용하는 것이 좋다
