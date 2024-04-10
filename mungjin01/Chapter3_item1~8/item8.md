# item8 타입 공간과 값 공간의 심벌 구분하기

타입스크립트에서 symbol은 타입 또는 값 둘 중 하나의 공간에 존재한다 그러니까 symbol의 이름이 같아도 어느 곳에 속하는 지에 따라 다른 것을 나타낼 수 있다는 말이다

```tsx
interface Cylinder {
  radius: number;
  height: number;
}

const Cylinder = (radius: number, height: number) => ({ radius, height });
```

interface Cylinder에서 Cylinder는 타입으로 쓰이고 const Cylinder에서 Cylinder는 값으로 쓰였지만 이 둘은 서로 아무런 관련이 없다

이러한 점이 가끔 오류를 야기하는데

```tsx
interface Cylinder {
  radius: number;
  height: number;
}

const Cylinder = (radius: number, height: number) => ({ radius, height });
function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape.radius;
    // ~~~~~~ Property 'radius' does not exist on type '{}'
  }
}
```

instanceof Cylinder는 타입을 체크해야 하지만, instanceof는 자바스크립트의 연산자이기에 값에 대한 참조를 진행한다. 그래서 instance of Cylinder는 타입이 아니라 함수를 참조한다

```tsx
class Cylinder {
  radius = 1;
  height = 1;
}

function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape; // OK, type is Cylinder
    shape.radius; // OK, type is number
  }
}
```

클래스가 타입으로 쓰일 때는 형태(속성과 메서드)가 사용되는 반면, 값으로 쓰일 때는 생성자가 사용된다

한편 연산자 중에서도 타입에서 쓰일 때와 값으로 쓰일 때 다른 기능을 하는 것이 있는데 그 중 하나가 typeof이다

```tsx
type T1 = typeof p; // Type is Person
type T2 = typeof email;
// Type is (p: Person, subject: string, body: string) => Response

const v1 = typeof p; // Value is "object"
const v2 = typeof email; // Value is "function"
```

타입의 관점에서 typeof는 값을 읽어서 타입스크립트 타입을 반환한다

값의 관점에서 typeof는 대상 심벌의 런타임 타입을 가리키는 문자열을 반환한다

여기에 class 타입을 적용시켜보

```tsx
class Cylinder {
  radius = 1;
  height = 1;
}

function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape; // OK, type is Cylinder
    shape.radius; // OK, type is number
  }
}
const v = typeof Cylinder; // Value is "function"
type T = typeof Cylinder; // Type is typeof Cylinder

declare let fn: T;
const c = new fn(); // Type is Cylinder

type C = InstanceType<typeof Cylinder>;
```

class는 자바스크립트에서 함수로 구현되니까 값으로 읽을 때 function을 반환한다 하지만 타입으로 읽을 때 인스턴스의 타입으로 읽지 않아서 InstanceType 제네릭을 사용해야 한다

아니 너무 어려움…
