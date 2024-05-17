# item 17. 변경 관련된 오류 방지를 위해 readonly 사용하기

```tsx
function printTriangles (n: number){
	const nums = [];
	for(let i = 0; i <n ; i++){
		nums.push(i);
		console.log(arraySum(nums));
		}
}

function arraySum (arr: number[]){
	let sum = 0, num;
	while ((num = arr.pop()) !== undefined){
		sum += num;
	}
	return sum;
}
```

⇒ arraySum이 nums를 변경하지 않는다고 간주해서 오류 발생

 ⇒ readonly  접근 제어자를 사용하여 arraySum이 배열을 변경하지 않는다는 선언

`function arraySum (arr : readonly number[])` 로 변경하면 

readonly number[] 형식에 pop 속성이 없다는 오류

 readonly number[]  = 타입이고, number [] 과 구분되는 몇 가지 특성

- 배열의 요소를 읽을 수 있지만, 쓸 수는 없다
- length를 읽을 수 있지만, 바꿀 수 없다(배열을 변경함)
- 배열을 변경하는 pop을 비롯한 다른 메서드를 호출할 수 없다.

number[]가 readonly number[]보다 기능이 많기 때문에, readonly number[]의 서브타입이다

→ 변경 가능한 배열을 readonly 배열에 할당 가능하지만, 

readonly 배열을 변경 가능한 배열에 할당할 수 없다

매개변수를 readonly로 선언하면 

- 타입스크립트는 매개변수가 함수 내에서 변경이 일어나는지 체크한다
- 호출하는 쪽에서 함수가 매개변수를 변경하지 않는다는 보장을 받게된다
- 호출하는 쪽에서 함수에 readonly 배열을 매개변수로 넣을수도 있다

함수가 매개변수를 변경하지 않는다면, readonly로 선언해야 한다

- 장점
    - 더 넓은 타입으로 호출할 수 있고,
    - 의도치 않은 변경이 방지된다
    - 지역 변수와 관련된 모든 종류의 변경 오류를 방지할 수 있다
- 단점
    - 매개변수가 readonly로 선언되지 않은 함수를 호출해야 할 경우도 있다

readonly는 얕게 동작한다. 

ex ) 객체의 readonly 배열 → 객체 자체는 readonly 가 아니다

Readonly  제너릭도 동일하게 동작.

깊은 readonly 타입 → ts-essentials에 있는 DeepReadonly 제너릭을 사용하자