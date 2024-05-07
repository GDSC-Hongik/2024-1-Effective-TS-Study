# item 11. 잉여 속성 체크의 한계 인지하기

타입이 명시된 변수에 객체 리터럴을 할당할 때 타입스크립트는 해당 타입의 속성이 있는지, 그리고 ‘그 외의 속성은 없는지’ 확인한다

```tsx
interface Room{
	numDoors : number;
	ceilingHeightFt : number;
}
const r:Room = {
	numDoors:1,
	ceilingHieghtFt: 10,
	elephant : 'present',
}; // 오류
```

→ 할당 가능하나, 잉여 속성 체크가 수행됨.

```tsx
const obj = {
	numDoors:1,
	ceilingHeightFt :10,
	elephant : 'present',
};
const r : Room = obj; //정상!
```

→ 할당 가능하며, 잉여 속성 체크가 수행되지 않음.

1. 잉여 속성 체크는 조건에 따라 발생하지 않는 경우가 있다
    1. 변수에 할당하는 게 객체 리터럴이 아닌 경우
    2. 타입 단언문을 사용하는 경우
2. 잉여 속성 체크가 할당 가능 검사와는 별도의 과정
3. 잉여 속성 체크를 이용하면 구조적 본질을 해치지 않으면서도 객체 리터럴에 알 수 없는 속성을 허용하지 않음으로서, 너무 큰 범위 같은 문제를 방지할 수 있다.