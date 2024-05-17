# item10. 객체 래퍼 타입 피하기

기본형 type - 불변이며, 관련 메서드를 가지지 않는 다는 점에서 객체와 구분됨

하지만 string : 메서드를 가지고 있는 것처럼 보임

ex) `‘primitive’.charAt(3)` 

하지만 이는 메서드가 아니고, 내부적으로 많은 동작이 일어나는 것이다.

자바스크립트는 기본형과 객체 타입을 서로 자유롭게 변환한다. 

string(기본형)에 메서드를 사용할 때, 자바스크립트는 

1. 기본형을 String객체로 wrapping 하고,
2.  메서드를 호출하고,
3.  마지막에 wrapping 한 객체를 버림

하지만 string 기본형과 객체가 항상 동일하게 동작하지는 않는다.

String 객체 - 자기 자신하고만 동일함

```tsx
"hello" === new String("hello") //false
new String("hello") === new String("hello") //false
```

**객체 래퍼타입의 자동 변환의 예외 동작**

- 어떤 속성을 기본형에 할당하면 그 속성이 사라짐
    
    ```tsx
    x = "hell0"
    x.language = 'English'
    x.language //undefined
    ```
    
    → x가 String 객체로 변환된 후 language 속성이 추가되었고, language 속성이 추가된 객체가 버려짐
    

**기본형들의 래퍼 타입**

- string - String
- number - Number
- boolean - Boolean
- symbol - Symbol
- bigint - BigInt

타입스크립트는 기본형과 객체 래퍼타입을 별도로 모델링함

**string을 유의하자**

string을 매개변수로 받는 메서드에 String 객체를 전달 시 문제가 발생함

기본형을 객체에 할당할 수 있지만, 객체는 기본형에 할당할 수 없음

객체 래퍼 타입 사용을 지양하고, 기본형 타입을 사용하자.