# 클래스

### 클래스의 상속

```tsx
class Pserson {
  construcrtor(name, skill) {
    this.name = name;
    this.skill = skill;
  }
  sayHi() {
    console.log("hi");
  }
}
class Developer extends Person {
  constructor(name, skill) {
    super(name, skill);
  }

  coding() {
    console.log("coding");
  }
}
```

여기 이 코드에서 Developer 클래스는 Person 클래스를 상속받는다

Developer 클래스의 생성자를 보면 super 키워드를 사용했는데 이 코드는 자식 클래스 (Developer 클래스)에서 new 키워드로 객체를 생성할 때 부모 클래스(Person)의 생성자를 호출하겠다는 뜻이다

그럼 Developer 클래스로 새로운 객체(클래스 인스턴스)를 하나 생성해보면

```tsx
let capt = new Developer('capt', 'typescript);
capt.coding(); // 'coding'
```

Developer 클래스로 생성한 객체를 생성하고 coding()를 호출하면 ‘coding’이 출력된다

여기서 capt변수로 부모 클래스의 메서드를 호출하려면 이렇게 하면 된다

```tsx
capt.sayHi(); //hi
```

name과 skill 속성에도 모두 접근가능하다

```tsx
console.log(capt.name); //capt
console.log(capt.skill); //typescript
```

### 타입스크립트의 클래스

```tsx
class Chatgpt {
  constructor(name: string) {
    this.name = name;
  }
  sum(a: number, b: number): number {
    return a + b;
  }
}
```

생성자 파라미터에 string 타입을 정의했고, sum() 메소드의 파라미터와 반환타입 모두 number로 정의했다. 그렇지만 막상 실행을 해보면 ‘name’ does not exist on type ‘Chatgpt’라고 뜬다

그 이유는 타입스크립트로 클래스를 작성할 때 생성자 메서드에서 사용될 클래스 프로퍼티들을 미리 정의해주어야 하기 때문이다

```tsx
class Chatgpt {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sum(a: number, b: number): number {
    return a + b;
  }
}
```

---

레퍼런스

- 쉽게 시작하는 타입스크립트
