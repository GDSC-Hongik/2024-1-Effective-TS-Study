# item3 코드 생성과 타입이 관계없음을 이해하기

컴파일 순서는 다음과 같다

소스 코드 컴파일 → 이를 기반으로 타입 체크 → 타입 체크를 통과한 코드를 자바스크립트 코드로 변환 →

타입스크립트 컴파일러는 두 가지 역할을 수행한다

- 브라우저에서 동작할 수 있도록 구버전의 자바스크립트로 트랜스파일(transfile)한다
- 코드의 타입 오류를 체크한다

이 두 가지는 서로 완벽히 독립적인데 다시 말하면 타입스크립트가 자바스크립트로 변환될 때 코드 내의 타입에는 영향을 주지 않고 자바스크립트의 실행 시점에도 타입은 영향을 미치지 않는다

### 타입 오류가 있는 코드도 컴파일이 가능하다

- 컴파일은 타입 체크와 독립적으로 동작하기 때문에 타입 오류가 있는 코드도 컴파일이 가능하다

### 런타임에는 타입 체크가 불가능하다

```tsx
interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    // ~~~~~~~~~ 'Rectangle' only refers to a type,
    //           but is being used as a value here
    return shape.width * shape.height;
    //         ~~~~~~ Property 'height' does not exist
    //                on type 'Shape'
  } else {
    return shape.width * shape.width;
  }
}
```

instanceof 체크는 런타임에 일어나지만, Rectangle은 타입이기 때문에 런타임 시점에 아무런 역할을 할 수 없다

앞의 코드에서 다루고 있는 shape 타입을 명확하게 하려면, 런타임에 타입 정보를 유지하거나 height 속성이 존재하는지 체크를 해야한다.

```tsx
interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;
function calculateArea(shape: Shape) {
  if ("height" in shape) {
    shape; // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape; // Type is Square
    return shape.width * shape.width;
  }
}
```

속성 체크는 런타임에 접근 가능한 값에만 관련되지만, shape의 타입을 Rectangle로 보정해주기 때문에 오류가 사라진다

### 타입 연산은 런타임에 영향을 주지 않는다

### 런타임 타입은 선언된 타입과 다를 수 있다

```tsx
function turnLightOn() {}
function turnLightOff() {}
function setLightSwitch(value: boolean) {
  switch (value) {
    case true:
      turnLightOn();
      break;
    case false:
      turnLightOff();
      break;
    default:
      console.log(`I'm afraid I can't do that.`);
  }
}
```

- boolean이 타입스크립틍에서 타입 선언문에 있으면 런타임 때 제거된다고 한다

### 타입스크립트 타입으로는 함수를 오버로드할 수 없다

C++ 같은 언어는 동일한 이름에 매개변수만 다른 여러 버전의 함수를 허용하는데 이것을 함수 오버로딩이라고 한다. 하지만 타입스크립트에서는 타입과 런타임의 동작이 무관하기 때문에 함수 오버로딩이 불가능하다
