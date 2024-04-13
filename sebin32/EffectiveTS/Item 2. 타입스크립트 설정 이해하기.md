# Item 2. 타입스크립트 설정 이해하기

타입 체커 설정하기

방법 1> CLI 이용

```bash
$ tsc --noImplicitAny program.ts
```

방법 2>tsconfig.json 설정 파일 작성 ⇒ 권장 ( 동료도 컴파일러의 설정을 알 수 있으므로 )

```bash
{
	"compilerOptio며n":{
		"noImplicitAny": true,
		"strictNullChecks": true,
	}
}
```

noImplicitAny : 변수의 타입을 설정하지 않으면 any로 간주하기 때문에, 해당 옵션을 true로 설정하면  모든 변수의 타입이 미리 정의되어야 한다.

```tsx
const add = (a,b) => {
	return a+b;
} // 해당 경우 오류 발생, 아래와 같은 수정이 필요

const add1 : number = (a: number, b: number) => {
	return a + b;
}
```

strictNullChecks :  null과 undefined가 모든 타입에서 허용되는지 확인하는 설정, noImplicitAny가 true로 설정되어 있어야 함.

```tsx
const x : number = null; // true로 설정된 경우 오류, false인 경우 유효한 값
const z : number | null = null; // 해당 경우는 true여도 오류 발생 x
```

그러나, 기존의 JS의 코드를 마이그레이션하는 경우라면, strictNullChecks는 설정하지 않아도 됨