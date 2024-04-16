# Item 8. 타입 공간과 값 공간의 심벌 구분하기

타입 공간 & 값 공간

```tsx
interface Cylinder {
  radius: number;
  height: number;
}  //타입 공간

const Cylinder= (radius: number, height: number) => ({radius,height}) //값 공간

```

⇒ type 이나 interface 다음에 나오는 심벌은 타입, const, let 다음에 나오는 심벌은 값

⇒ 타입 선언 ( : ) 또는 타입 단언 (as) 뒤는 타입, 할당 연산자 (=) 뒤는 값

class 와 enum은 상황에 따라 타입과 값 모두 가능한 예약어 

```tsx
class Cylinder {
	radius = 1;
	height = 1;
} 

function calculateVolume (shape : unknown){
	if ( shape instanceof Cylinder) { 
		shape 
		shape.radius
	}
}
```

해당 코드는 class가 타입으로 쓰인 경우

타입과 값에서 다르게 쓰이는 연산자 typeof 

```tsx
//타입의 관점에서 typeof - TS의 타입 반환
type T1 = typeof p; // 타입은 Person
type T2 = typeof email;
// 타입은 (p: Person, subject: string, body: string) => Response

//값의 관점에서 typeof -  JS의 런타임 타입 반환 (string, number, boolean, undefined, object, function)
const V1 = typeof p;  // 값은 'object'
const V2 = typeof email; // 값은 'function'
```

```tsx
const v = typeof Cylinder //'function' - 클래스가 js에서는 함수로 구현되기 때문에
type T = typeof Cylinder // 타입이 typeof Cylinder
//인스턴스의 타입이 아님, new 키워드를 사용할 때 볼 수 있는 생성자 함수
```

```tsx
declare let fn: T;
const c = new fn(); // type이 Cylinder

type C = InstanceType<typeof Cylinder>; //타입이 Cylinder
```

속성에 접근하는 방법에 따라 타입일 수도, 값일 수도 있다.

```tsx
const first : Person['first'] = p['first'] | p.first;
//Person['first'] 는 타입, p['first'] | p.first는 

type PersonEl = Person['first'|'last']; //타입은  string
type Tuple = [string,number,Date];
type TupleEl = Tuple[number] //타입은 string | number | Date
```

타입 관점에서 속성에 접근할 때에는 obj[’field’]로 접근해야 하고, 값 관점에서 속성에 접근할 때에는 obj[’field’]와 obj.field 모두 가능

타입 공간과 값 공간의 차이가 있는 코드 패턴들

- this ( js의 this vs 다형성 this )
- &, |  ( and or 비트 연산자 vs 인터섹션, 유니온 )
- const ( 새 변수 vs as const 리터럴의 추론된 타입 변환  )
- extends ( 서브클래스, 서브타입, 제네릭 타입의 한정자 정의)
- in