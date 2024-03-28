# Section02. Javascript 심화

# Truthy와 Falsy

Javascript의 모든 값은 Truthy 하거나 Falsy하다

## Falsy한 값

undefined, null, 0, -0, NaN, “”, 0n

## Truthy한 값

: falsy 제외 모든 값

문자열, 빈 배열, 빈 객체, 함수,…

# 단락 평가

: 첫번째 피연산자값 만으로도 연산의 결과를 확정할 수 있다면 두 번째 피연산자는 접근조차 안한다!

```jsx
function printName(person){
	const name = person && person.name;
	console.log(name || "person의 값이 없음");
}
printName(); // "person의 값이 없음"
printName({name: "이정환"}); //이정
```

Truthy1 || Truthy2 → Truthy1

Truthy1 && Truthy2 ⇒ Truthy 2 

# 구조 분해 할당

## 배열의 구조 분해 할당

: 배열의 원소들을 순서대로 하나씩 분해해서 할당하는 문법

```jsx
let arr = [1,2,3,4];
let [one,two,three,four = 4] = arr;
```

## 객체의 구조 분해 할당

: 객체의 프로퍼티를 키값을 기준으로 변수에 할당하는 문법

- 중괄호를 이용

```jsx
let person = {name:"이정환", age :27, hobby: "테니스" };

//할당받는 변수의 이름 변경 가능
let {age:myAge, hobby,name,extra = "hello"}= person;
```

## 객체 구조 분해 할당을 이용해서 함수의 매개변수를 받는 방법

```jsx
const func = ({name, age, hobby, extra})=>{
	console.log(name,age,hobby,extra);
};

func(person);
```

- 주의할 점 !

: 객체를 넘겼을 때만 구조분해할당이 가능한 것!

# Spread 연산자 & Rest 매개변수

## Spread 연산자 …

: 객체나 배열에 저장된 여러개의 값을 개별로 흩뿌려주는 역할

```jsx
let arr1 = [1,2,3];
let arr2 = [4,...arr1,5,6];; //4,1,2,3,4,5,6

let obj1 = {
	a:1,
	b:2,
};
let obj2 = {
	...obj1,
	c: 3,
	d: 4,
};

function funcA(p1,p2,p3){
}
funcA(...arr1);
```

## Rest 매개변수, 나머지 매개변수

: 여러 개의 매개변수를 받아야 할 때, **배열형태로** 한번에 받아올 수 있게 하는 변수

```jsx
function funcB(...rest){
	console.log(rest);
}
funcB(...arr1);
//spread연산자로 인수를 rest매개변수에 전달
```

- …이 매개변수를 정의하는 공간인 소괄호 안에 사용됨
    
     → spread연산자가 아님! 둘은 엄연히 다른 것임!
    

```jsx
	function funcB(one,...rest){
	console.log(rest);
}
-> rest [2,3]
funcB(...arr1);

//반드시 rest 매개변수가 마지막에 오도록 설정해야함
functino funcB(one, ...rest,four )  //안됨!
```

- 변수 이름은 맘대로 지정 가능!

# 원시타입 vs. 객체타입

![Untitled](Section02%20Javascript%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20e7662a976fbc4137adea0097e3903249/Untitled.png)

> 타입을 왜 나눴을까?
→ 값이 **저장되거나 복사되는 과정이 서로 다르기 때문**!
> 

> 원시타입 : 값 자체로써 변수에 저장되고 복사된다
> 

> 객체타입 : 참조값을 통해 변수에 저장되고 복사된다
> 

## 원시타입

⇒ 불변값 : 실제 메모리에서의 값이 수정되지 않는다는 뜻의 불변

## 객체타입

⇒ 가변값 : 메모리에 저장되어있는 원본데이터 값을 수정해버림

변수의 메모리에는 객체 값이 저장된 메모리 공간을 가르키는 주소값이 저장되어있음

## 객체타입 주의사항 1. 의도치 않은 값의 수정

### 얕은 복사

: 객체의 참조값을 복사함

```jsx
let o1 = {name : '이정환'};
let o2 = o1;
```

### 깊은 복사

: 새로운 객체를 생성하면서 프로퍼티만 따로 복사함

→ 원본 객체가 수정될 일이 없어 안전함!

```jsx
let o1 = {name: "이정환"};
let o2 = {...o1};
```

## 객체타입 주의사항 2. 비교 - 참조값 기준!(얕은 비교)

- 깊은 복사 시, 참조값이 달라짐.
- 실제 값은 같더라도, 참조값이 달라서 다르다고 평가

참조값이 아닌 프로퍼티를 기준으로 두 객체를 비교하고 싶다면,
→ **`JSON.stringify()` 로 객체→ 문자열 형변환 (깊은 비교)**

```jsx
JSON.stringify(o1) === JSON.stringify(o2);
```

## 객체타입 주의사항 3. 배열도 함수도 사실 객체

객체처럼 추가적인 프로퍼티나 메서드를 가질 수 있다

# 반복문으로 배열과 객체 순회하기

## 배열 순회

### 1. 배열 인덱스

```jsx
let arr = [1,2,3];
for(let i = 0; i<arr.length; i++){
}
```

### 2. for of 반복문

```jsx
for (let item of arr){
	console.log(item);
}
```

차이점

- index : for문 안에서 index를 통한 다른 활동 가능
- for of는 index를 따로 저장 x

## 객체 순회

### 1. Object.keys 사용

: 객체에서 key값들만 뽑아서 새로운 배열로 변환

```jsx
let person = {name:"이정환", age :27, hobby: "테니스" };
let keys = Object.keys(person);
for (let i = 0; i <keys.length; i++){
}
//or
for (let key of keys){
	console.log(key, person[key]);
}
```

### 2. Object.values 사용

: 객체에서 value값들만 뽑아서 새로운 배열로 변환

```jsx
let values = Object.values(person);
```

### 3. for in

: 객체의 property의 key를 순서대로 key에 할당

```jsx
for (let key in person){}
```

<aside>
💡 객체 - in! 배열 - of!

</aside>

# 배열 메서드

## 1. 요소 조작

1. `push`: 배열의 맨 뒤에 새로운 요소 추가
- 반환값 : push 메서드 수행 후의 배열의 길이
1. `pop`: 배열의 맨 뒤에 요소 제거, 반환
2. `shift` : 배열의 맨 앞에 있는 요소 제거, 반환
3. `unshift`: 배열의 맨 앞에 새로운 요소 추가
- 반환값 : 메서드 수행 후의 배열의 길이

> shift, unshift는 맨 앞에 추가/ 제거 → 인덱스 조작→ push, pop보다 느림!!
> 
1. `slice`: 배열의 특정 범위를 잘라내서 새로운 배열로 반환
    - `arr.slice(i, j+1)` : i~j까지 인덱스를 잘라서 새로운 배열로 반환
    - `arr.slice(2)` : 인덱스 2번부터 끝까지 잘라서 반환
    - 뒤에서부터 자르고 싶다면 : 음수값!
2. `concat` : 두 개의 서로 다른 배열을 이어 붙여서 새로운 배열을 반환

## 2. 순회와 탐색

1. `forEach`

: 모든 요소를 순회하면서, 각각의 요소에 특정 동작을 수행

콜백함수를 넣어주자

- 콜백함수는 배열의 길이만큼 호출됨
- 콜백함수의 매개변수에 item, idx, arr가 전달

```jsx
let arr1 = [1,2,3];
arr1.forEach(function(item, idx, arr){})

//ex
let doubledArr = [];
arr1.forEach((item)=>{
	doubledArr.push(item*2);
});
console.log(doubledArr); // [2,4,6[

```

1. 탐색 - `includes`

: 배열에 특정 요소가 있는지 확인하는 메서드

- boolean 값 반환
1. `indexOf`

: 특정 요소의 인덱스를 반환하는 메서드

- 없다면 → -1반환
1. `findIndex`

: 모든 요소를 순회하면서, 콜백함수를 만족하는 특정 요소의 인덱스를 반환하는 메서드

- 콜백함수를 만족? → 콜백함수가 참을 반환한다는 것
- 가장 처음으로 만족하는 요소의 인덱스를 반환
- 없다면 → -1 반

```jsx
let arr = [1,2,3];
arr4.findIndex((item)=>{
	if(item %2 !==0 ) return true;
}); 
// ===
const findIndx = arr4.findIndex((item)=>item % 2 !==0); 
// 조건식의 결과를 반
```

### 왜 findIndex가 존재?

- **indexOf : 얕은 비교로 동작** → 객체 배열에서 특정 객체값이 존재하는지 찾아낼 수가 없음
- **findIndex : 콜백함수를 이용해서 직접 특정 프로퍼티의 값을 기준으로 비교할 수 있음!**
1. `find`

: 모든 요소를 순회하면서 **콜백함수**를 만족하는 요소를 찾는데, **요소를 그대로 반환**

## 3. 배열 변형

1. `filter`

: 기존 배열에서 콜백함수로 전달한 조건을 만족하는 요소들만 필터링하여 새로운 배열로 반환

```jsx
const tennisPeople = arr1.filter((item)=>item.hobby ==="테니스");
```

1. `map`

: 배열의 모든 요소를 순회하면서, 각각 콜백함수를 실행하고 그 결과값들을 모아서 새로운 배열로 반환

- 반환값을 설정할 수 있음

```jsx
let arr2 = [1,2,3];
const mapResult = arr2.map((item,idx,arr)=>{
	return (item*2);
});
```

- 객체 배열의 프로퍼티 중 하나만 골라서 추출

```jsx
let names = arr1.map((item)=>item.name);
```

1. `sort`

: 배열을 **사전** 순으로 정렬

```jsx
arr3 = [10,3,5];
arr3.sort();
console.log(arr3); // [10,3,5]
```

1. `toSorted`

: 원본 배열은 그대로 두고, 정렬된 새로운 배열을 반환

1. `join("구분자")`

: 배열의 모든 요소를 하나의 문자열로 합쳐서 반환하는 메서드

```jsx
let arr6 = ["hi", "im","winterlood"];
const joined = arr6.join();
console.log(joined); // hi,im,winterlood
```

# Date객체와 날짜

## 1. Date 객체 생성 : 생성자

```jsx
let date1 = new Date();
let date2 = new Date("년/월/일/시:분:초");
```

## 2. 타임 스탬프

: 특정 시간이 UTC로부터 몇 ms가 지났는지를 의미하는 숫자값

- UTC : 1970.01.01 00시 00분 00초

```jsx
let ts1 = date1.getTime();

//새로운 데이트 객체 생성에도 타임프 스탬프 사용가능
let date4 = new Date(ts1); 
```

## 3. 시간 요소들을 추출하는 방법

```jsx
let year = date1.getFullYear();
let month = date1.getMonth()+1; //js의 month는 0부터 시작!
let date = date1.getDate();
let hour = date1.getHours();
let minute = date1.getMinutes();
let second = date1.getSeconds();
```

## 4. 시간 수정하기

```jsx
date1.setFullYear(2023);
date1.setMonth(2); //3월
date1.setDate(30);
setHours
setMinutes
setSeconds
```

## 5. 시간을 여러 포맷으로 출력하기

`.toDateString()` : 문자열 (Thu Mar 30 2023)

`.toLocaleString()` : 우리나라 시간(2023.3.30 오후 11:59:59)

# 동기와 비동기

**js는 동기적으로 코드를 실행한다**

- 비동기 : 순서대로 처리하지 않는 것.
- 각 작업의 결과값을 이용하고 싶다면 → 콜백함수를 붙여서 처리
- 비동기함수 : `setTimeout()`
- 비동기 작업들은 js엔진이 아닌 Web APIs에서 실행됨

## js의 비동기 처리

→ Web APIs 사용

### Web APIs

: 웹 브라우저가 직접 관리하는 별도의 영역

# 비동기 작업 처리하기 - 1. 콜백함수

```jsx
function add(a,b,callback){
	setTimeOut(()=>{
		const sum = a+b;
		callback(sum);
	},3000);
	
add(1,2,(value)=>{
	console.log(value);
};
```

비동기 작업을 하는 함수의 결과 값을 이 함수의 외부에서 이용하고 싶다면 → 매개변수로 콜백함수를 받아서,  비동기 함수 안에서 콜백함수를 호출하여 결과값을 넘겨주도록 설정!

# 비동기 작업 처리하기 - 2. Promise

**Promise** : 비동기작업을 효율적으로 처리할 수 있도록 도와주는 js 내장 객체

- 비동기 작업을 감싸는 객체

### promise의 3가지 상태

- pending
- fulfilled
- rejected

### Executor

: promise 객체를 생성할 때, 인수로 전달되는 콜백함수.(비동기 작업을 진행하는 코드) 

### then 메서드

`promise.then(()⇒{});`

- executor 함수에서 resolve를 호출하게 되면 그 후에 then에 전달한 콜백함수가 실행됨.
- resolve의 결과값을 then 콜백함수의 매개변수로 전달

### catch 메서드

`promise.catch(()⇒{});`

- executor 함수에서 reject 호출시 콜백함수 실행
- reject의 결과 값을 catch 콜백함수의 매개변수로 전달

### Promise Chaining

then의 결과 값 = promise 객체 → 연달아 then, catch를 사용해도 됨

### 동적으로 매개변수 받아서 promise 객체 생성하기

```jsx
function add10(num){
	const promise = new Promise((resolve,reject)=>{
		setTimeOut(()=>{
			if(typeof num === "number"){
				resolve(num+10);
			} else{
				reject("num이 숫자가 아닙니다");
			}
		},2000);
	});
	return promise;
}

//콜백 지옥을 방지하기 위해서!
const p = add(10);
p.then((result)=>{
	console.log(result);
	
	const newP = add10(result); //새로운 promise객체를 반환해주면
	newP.then((result)=>{
		console.log(result);
	});
	return newP;
});

//->
const p = add(10);
p.then((result)=>{	
	const newP = add10(result); //새로운 promise객체를 반환해주면
	return newP;
}).then((result)=>{
		console.log(result);
});

//더 간결히 작성도 가능!
add10(0).then((result)=>{
	console.log(result);
	return add10(result);
}).then((result)=>{
	console.log(result);
});
```

- 원래 아무것도 return하지 않았다 → 원본 프로미스 객체 반환
- 새로운 promise 객체를 return하게 하면 → 전체 then 메서드의 결과 값이 새로운 프로미스 객체가 됨
- 콜백 지옥 없이 앞의 결과를 사용 가능하다~

# 비동기 작업 처리하기 - 3. Async/Await

## async

: 어떤 함수를 비동기 함수로 만들어주는 키워드

함수가 **프로미스를 반환하도록 변환**해주는 키워드

- 애초에 해당 함수가 promise를 반환하고 있는 비동기 함수였다면, async가 별 일 안하고 그냥 promise 반환함
- 즉, async는 프로미스를 반환하지 않는 함수에 붙여서 자동으로 해당 함수를 비동기로 작동하도록 하는 기능

## await

: async 함수 내부에서만 사용이 가능한 키워드

비동기 함수가 다 처리되기를 기다리는 역할

- await 붙은 애가 다 끝나기까지 기다림