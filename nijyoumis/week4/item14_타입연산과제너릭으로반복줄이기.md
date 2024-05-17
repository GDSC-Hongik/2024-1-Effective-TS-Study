# item 14. 타입 연산과 제너릭 사용으로 반복 줄이기

1. **타입에 이름 붙이기**
    
    ```tsx
    function distance (a: {x:number,y:number}, b ;{x:number, y:number}) {
    	return Math.sqrt(Math.pow(a.x - b.x ,2) + Math.pow(a.y - b.y,2));
    }
    =>
    interface Point2D{
    	x: number;
    	y: number;
    }
    function distance(a:Point2D, b:Point2D){/*...*/}
    ```
    
2. **interface 확장하기 (extends)**
    
    
3. **이미 존재하는 타입을 확장하는 경우**
    
    일반적이진 않지만 인터섹션(&) 연산자를 사용할 수 있음
    
    ```tsx
    type PersonWithBirthDate = Person & {birth: Date};
    ```
    
4. **타입 매핑하기**
    - 전체 애플리케이션 상태를 표현하는 State와, 부분을 표현하는 TopNavState가 있는 경우,
        
        TopNavState를 State의 부분집합으로 정의하여 전체 앱의 상태를 하나의 인터페이스로 유지
        
        ```tsx
        type TopNavState = {
        	userId : State['userId'];
        	pageTitle : State['pageTitle'];
        	recentFiles : State['recentFiles'];
        };
        ```
        
        매핑된 타입 → 배열의 필드를 루프 도는 것과 같은 방식 
        
        ```tsx
        type TopNavState = {
        	[k in 'userId' | 'pageTitle' | 'recentFiles'] :State[k]
        };
        ```
        
        - 표준 라이브러리의 `Pick`과 동일
        - pick은 제너릭 타입이다
        
        ```tsx
        type Pick<T,K> = {[k in K]: T[k] };
        ```
        

- 단순히 태그를 붙이기 위해서 타입을 사용하면 다른 형태의 중복이 발생

```tsx
interface SaveAction {
	type: 'save';
	//...
}

interface LoadAction {
	type : 'load';
	//...
}
type Action = SaveAction | LoadAction;
type ActionType = 'save' | 'load'; //타입의 반복
```

1. **유니온을 인덱싱**

```tsx
type ActionType = Action['type']; 
```

<aside>
💡 **Pick vs Union Indexing**
- `Pick`은 해당 type 속성을 가지는 인터페이스가 됨 // {type: “save” | “load”}
- 유니온 인덱싱은 그냥 유니온된 타입! // “save” | “load”

</aside>

1. **생성 후 업데이트가 되는 클래스 정의 → 매핑된 타입과 keyof 사용**

```tsx
type OptionsUpdate = {[k in keyof Options]?: Options[k]};
```

- keyof는 타입을 받아서 속성 타입의 유니온 반환
    
    ```tsx
    type Optionskeys = keyof Options; // width|height|color|label
    ```
    
- 표준 라이브러리의 `Partial`과 동일

1. **값의 형태에 해당하는 타입을 정의하고 싶을 때 → `typeof` 사용**
    
    ```tsx
    type Options = typeof INIT_OPTIONS;
    ```
    
    타입 정의를 먼저 하고, 값이 그 타입에 할당 가능하다고 선언하는 것이 좋다
    
2. **함수나 메서드의 반환값에 명명된 타입을 만들고 싶을 때 → 조건부 타입 / `ReturnType`** 

```tsx
type UserInfo = ReturnType<typeof getUserInfo>;
```

→ 함수의 ‘값’이 아니라 함수의 ‘타입’인 typeof getUserInfo 를 사용해야함.

1. **제너릭 타입에서 매개변수를 제한하는 방법 - extends** 

제너릭 타입은 타입을 위한 함수이다 → 함수에서 매개변수를 제한하듯이, 제너릭 타입의 매개변수도 제한할 방법이 필요!

```tsx
interface Name {
	first : string;
	last : string;
}
type DancingDuo<T extends Name> =[T,T];
const couple1 : DancingDuo<Name> =[
	{first:'Fred', last: 'adfsd'},
	{first:'Alex', last: 'dfdf'},
]; //ok

const couple2 : DancingDuo<Name> =[
	{first:'Fred'},
	{first:'Alex'},
]; // last 속성이 없다는 오류
```

1. **pick과 extends**

```tsx
type Pick<T,K> = {[k in K]: T[k] }; //오류 발생
```

→ K는 T 타입과 무관, k는 T의 부분집합 , keyof T가 되어야 한다

```tsx
type Pick<T,K extends keyof T> = {[k in K]: T[k] }; //ok!
```

*extends를 확장이 아니라 ‘부분집합’의 개념으로 이해하자!*