# item 18. 매핑된 타입을 사용하여 값을 동기화하기

scatter plot을 그리기 위한 UI 컴포넌트를 작성한다고 가정해보자

```tsx
interface ScatterProps{
	xs : number[];
	ys : number[];
	
	xRange : [number, number];
	yRange : [number, number];
	color : string;
	
	onClick : (x: number, y: number, index : number) => void;
}
```

**최적화 방법**

1. 보수적(conservative) 접근법 , 실패해 닫힌(fail close) 접근법

```tsx
function shouldUpdate(
	oldProps : ScatterProps,
	newProps : ScatterProps
){
	let k : keyof ScatterProps;
	for (k in oldProps){
		if (oldProps[k] ! == newProps[k]){
			if( k !== 'onClick') return true;
		}
	}
	return false;
}
```

→ 새로운 속성이 추가되면 값이 변경될 때마다 차트를 다시 그림

1. 실패에 열린 접근법

```tsx
function shouldUpdate(
	oldProps : ScatterProps,
	newProps : ScatterProps
){
	return(
		oldProps.xs !== newProps.xs ||
		oldProps.ys !== newProps.ys ||
		oldProps.xRange !== newProps.xRange ||
		oldProps.yRange !== newProps.yRange ||
		oldProps.color !== newProps.color
		);
}
```

→ 누락되는 일이 생길 수 있음

1. 타입 체커가 동작하도록  - 매핑된 타입, 객체 사용

```tsx
const REQUIRES_UPDATE: {[k in keyof ScatterProps] : boolean} = {
	xs: true,
	ys: true,
	xRange: true,
	yRange : true,
	color : true,
	onClick: false,
};

function shouldUpdate(
	oldProps : ScatterProps,
	newProps : ScatterProps
){
	let k : keyof ScatterProps;
	for (k in oldProps){
		if (oldProps[k] ! == newProps[k] && REQUIRES_UPDATE[k]){
			return true;
		}
	}
	return false;
}
```

매핑된 타입은 한 객체가 또 다른 객체와 정확히 같은 속성을 가지게 할 때 이상적