# Item 4. 구조적 타이핑에 익숙해지기

TS는 JS와 같이 매개 변수 값이 요구 사항을 만족한다면 타입이 무엇인지 신경 쓰지 않는 동작을 그대로 모델링하는 덕 타이핑 기반

```tsx
interface Vector2D {
  x: number;
  y: number;
}

function calculateLength( v: Vector2D ) {
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

interface NamedVector {
  name : string;
  x : number;
  y : number;
}

//아래 코드에서 에러 발생 x

const v : NamedVector = { x:3, y:4, name: 'zee'}
calculateLength(v); // 결과는 5
//NamedVector와 Vector2D가 호환되는 것 = 구조적 타이핑 때문 
```

이번엔 구조적 타이핑 때문에 에러가 발생하는 경우 

```tsx
interface Vector2D {
  x: number;
  y: number;
}

interface Vector3D {
  x: number;
  y: number;
  z: number;
}

function calculateLength( v : Vector2D ){
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

function normalize( v: Vector3D ) {
  const length = calculateLength(v);
  return {
    x: v.x / length,
    y: v.y / length,
    z: v.z / length,  
  };
}

//아래 코드에서 에러 발생
normalize({ x: 3, y: 4, z: 5})  
// 1이 나와야하지만, 1.41이 나옴. -> length가 Vector2D 기반으로 연산되기 때문
// z가 정규화에서 무시된 것.
```

클래스 구조적 타이핑 규칙을 따르기 때문에 유의해야 함.