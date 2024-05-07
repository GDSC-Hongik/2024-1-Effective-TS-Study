# item9. 타입 단언보다는 타입 선언을 사용하기

```yaml
interface Person {name : string};
const alice :Person = {name:'Alice'}; //타입 선언
const bob = {name :'Bob'} as Person; //타입 단언
```

타입 단언은 강제로 타입을 지정 → 타입 체커에 오류를 무시하라고 하는 것임

**화살표함수의 반환 타입 선언**

타입 단언 사용시 런타임에 에러 발생.

```yaml
const people = ['alice', 'bob', 'jan'].map(
	(name):Person =>({name})
);
```

(name): Person - name의 타입이 없고, 반환 타입이 Person

(name:Person) - name의 타입이 person임을 명시하고 반환 타입은 없어서 오류가 발생

```yaml
const people:Person[] = ['alice', 'bob', 'jan'].map(
	(name):Person =>({name})
);
```

⇒ 원하는 타입 직접 명시, 타입스크립트가 할당문의 유효성을 검사

**타입 단언이 꼭 필요한 경우**

1. 타입 체커가 추론한 타입보다 우리가 판단하는 타입이 더 정확할 때
- 타입스크립트는 DOM에 접근할 수 없기 때문에 #myButton이 버튼 엘리멘트인지 알지 못함.
1. 자주 쓰이는 특별한 문법(!)을 사용해서 null이 아님을 단언하는 경우
- 접미사로 쓰인 ! 는 그 값이 null이 아니라는 단언문으로 해석됨
    
    단언문은 컴파일 과정 중에 제거되므로, 타입 체커는 알지 못하지만 그 값이 null이 아니라고 확신할 수 있을 때 사용해야한다. 그렇지 않다면, null 인 경우를 체크하는 조건문을 사용해야함.
    

타입 단언문으로 임의의 타입간에 변환 불가