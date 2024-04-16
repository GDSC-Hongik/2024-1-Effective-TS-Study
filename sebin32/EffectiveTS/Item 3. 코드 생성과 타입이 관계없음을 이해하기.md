# Item 3. 코드 생성과 타입이 관계없음을 이해하기

타입스크립트 컴파일러의 역할

1> 최신 버전의 TS,JS가 브라우저에서 동작할 수 있도록 구버전의 JS로 트랜스파일 함.

2> 코드의 타입 오류 체크

⇒그러나 서로 독립적인 작업 → JS로 변환될 때 코드 내의 타입에는 영향 X & 변환된 JS가 실행될 때에도 코드 내 타입에는 영향 X

### 타입 오류가 있는 코드도 컴파일 가능

C와 자바 같은 정적 타입 시스템의 언어는 코드에 타입 오류가 있는 경우, 컴파일이 되지 않음 ( 타입 체크와 컴파일이 동시에 이뤄지기 때문에 ) 

그러나 TS는 타입 오류에 대한 경고만 표시할 뿐 빌드는 진행 됨. 

오류가 있을 때, 컴파일하지 않으려면 tsconfig.json 설정 파일에서 noEmitionError를 설정하면 됨.

### 런타임에는 타입 체크가 불가능

```tsx

interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;

function CalculateArea (shape : Shape ) {
  if (shape instanceof Rectangle) { 
  //해당 코드에서 Renctangle은 타입인데, 런타임에 타입 정보는 제거되기 때문에
  //오류 발생 
    return shape.width * shape.height;
	  //Square에는 height이라는 속성이 존재하지 않는다는 경고문
  }else {
    return shape.width * shape.width;
  }
}
```

아래와 같은 타입 정제가 필요

```tsx
interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;

function CalculateArea (shape : Shape ) {
  if ('height' in shape) {
    return shape.width * shape.height;
  }else {
    return shape.width * shape.width;
  }
}
```

```tsx
interface Square {
  kind: 'square';
  width: number;
}
interface Rectangle {
  kind: 'rectangle'
  width: number;
  height: number;
}
type Shape = Square | Rectangle;

function CalculateArea (shape : Shape ) {
  if (shape.kind ==='rectangle') {
    return shape.width * shape.height;
  }else {
    return shape.width * shape.width;
  }
}
```

```tsx
class Square {
  constructor(public width: number) {}
}
class Rectangle extends Square {
  constructor(public width: number, public height: number) {
    super(width);
  }
}
type Shape = Square | Rectangle;

function CalculateArea (shape : Shape ) {
  if (shape instanceof Rectangle) {
    return shape.width * shape.height;
  }else {
    return shape.width * shape.width;
  }
}
```

### 타입 연산은 런타임에 영향을 주지 않음

마찬가지로 런타임에선 JS 코드가 동작하는 거기 때문에 타입과 관련된 코드가 다 사라져서 타입 연산도 런타임에 영향을 미치지 않는 것

```tsx
function asNumber ( val: number | string ):number {
  return val as number;
} 
//이 경우에는 타입과 관련된 연산인 as Number가 js 코드에서 사라지므로 val가 그대로 반환됨

function asNumber *( val* : number | string ) : number {
  return typeof(val) === 'string' ? Number(val) : val ; 
}
```

### 런타임 타입은 선언된 타입과 다를 수 있음

```tsx
function setLightSwitch (value : boolean) {
  switch (value) {
    case true:
      turnLightOn();
      break;
    case false:
      turnLightOff();
      break;
    default:
      console.log('실행되지 않을까봐 걱정됩니다.');
  }
}// 이부분까지만 작성되어 있다면 마지막 default 부분의 코드는 실행되지 않음.
//boolean값만 파라미터로 받을 수 있기 때문에 

interface LightApiResponse {
  lightSwitchValue: boolean;
}

async function setLight() {
  const response = await fetch('/light');
  const result: LightApiResponse = await response.json();
  setLightSwitch(result.lightSwitchValue);
}
//그러나 이 경우에는 api response의 타입을 알 수 없기 때문에 
//런타임의 타입과 선언된 타입이 다를 수 있음
```

해당 경우에는 타입 스크립트에서도 마지막 줄이 실행 됨. 

### 타입 스크립트 타입으로는 함수를 오버로드할 수 없다.

함수 오버로딩 : 동일한 이름의 매개변수만 다른 함수를 허용하는 것

그러나 TS가 JS로 변환되는 경우, 타입과 관련된 코드가 모두 사라지기 때문에 결국 하나의 구현체(Implementation)만 남게 된다.

### 타입스크립트 타입은 런타임 성능에 영향을 주지 않는다.

TS 코드가 JS로 변환될 때 타입과 타입 연산자가 제거되기 때문에 런타임의 성능에는 영향을 주지 않음

⇒ 타입스크립트의 컴파일러는 런타임 오버헤드는 없지만, 빌드타입 오버헤드가 존재

오버헤드가 커지는 경우, 빌드 도구에서 트랜스파일만을 설정해서 타입 체크를 건너띌 수 있음.