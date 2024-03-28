# this 용법 (md)

### **상황에 따라 달라지는 this**

- 자바스크립트에서 this는 기본적으로 함수가 호출될 때 함께 결정된다.
  - 함수가 호출될때 생성되는 실행 컨텍스트는 생성과 동시에 this 바인딩 과정을 거치기 때문
  - 더 쉽게 말하면 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, **함수를 호출할 때 함수가 어떻게 호출되었는지에 따라** this에 바인딩할 객체가 동적으로 결정된다.

일반적으로 실행 컨텍스트는 함수를 호출할 때 생성되므로, **this는 함수를 호출할 때 결정**된다고 볼 수 있다. 하지만 **함수를 어떤 방식으로 호출하느냐에 따라 값이 달라지기 때문**에 각 상황별로 this의 값을 알아보면

### **전역 공간에서의 this**

```jsx
var test = 1;
console.log(test); // 1
console.log(window.test); // 1
console.log(this.test); // 1
```

위 예제에서 test를 직접 호출한 결과가 나오는 이유는, 변수 test에 접근하고자 하면, **스코프 체인**에서 test를 검색하다가 가장 마지막의 전역 스코프의 LexicalEnvironment, 즉 전역 객체에서 해당 프로퍼티인 test를 발견하여 값을 반환한다

(전역 변수 선언 시 자바스크립트 엔진은 이를 전역 객체의 프로퍼티로 할당한다. → 자바스크립트의 모든 변수는 실행 컨텍스트 LexicalEnvironment의 프로퍼티로서 동작)

### **메서드로서 호출하면 this는 호출한 주체(객체)에 대한 정보를 가리킨다**

함수를 실행하는 방법

1. 함수로서 호출하는 경우
2. 메서드로서 호출하는 경우

차이점: 독립성 (함수는 독립적으로 지능을 수행하지만, 메서드는 자신을 호출한 대상 객체에 관한 동작을 수행한다.)

**함수**

- 그 자체로 독립적인 기능을 수행

**메서드**

- 자신을 호출한 대상 객체에 관한 동작

```jsx
var obj = {
  methodA: function (x) {
    console.log(this, x);
  },
};

obj.methodA(1); // {method: ƒ} 1
obj["method"](2); // {method: ƒ} 2
```

```jsx
var obj = {
  methodA: function () {
    console.log(this);
  },
  inner: {
    methodB: function () {
      console.log(this);
    },
  },
};

// methodA 호출 주체는 obj
obj.methodA(); // obj
obj["methodA"](); // obj

// methodB 호출 주체는 inner
obj.inner.methodB(); // obj.inner
obj.inner["methodB"](); // obj.inner
obj["inner"].methodB(); // obj.inner
obj["inner"]["methodB"](); // obj.inner
```

메서드 내부에서의 this - this에는 호출한 주체(함수명, 프로퍼티명 앞의 객체)에 대한 정보가 담긴다 마지막 **점 앞에 명시된 객체가 곧 this**가 된다

### **함수로서 호출하면 this는 일반적으로 전역 객체를 가리킨다**

- 함수를 함수로서 호출할 경우 this가 지정되지 않는다. 실행 컨텍스트를 활성화할 당시에 this가 지정되지 않는 경우 this는 전역 객체를 바라보기 때문에, 함수에서의 this는 전역 객체를 가리킨다
- 메서드 내부의 함수에서도 함수로서 호출되는 경우는 전역 객체를 가리킨다

```jsx
var obj1 = {
  outer: function () {
    console.log(this); // {outer: ƒ}, 객체 obj1의 프로퍼티로 호출
    var innerFunc = function () {
      console.log(this); // Window, 함수로서 호출
    };
    innerFunc();

    var obj2 = {
      innerMethod: innerFunc, // {innerMethod: ƒ}, 객체 obj2의 프로퍼티로 호출
    };
    obj2.innerMethod();
  },
};
obj1.outer();
```

**_cf. 메서드와 함수 호출 구분하는 방법_**

함수는 그 자체로 독립적인 기능을 수행하는 것이고, 메서드는 자신을 호출한 대상 객체에 관한 동작을 수행하는 것으로 이 둘의 호출 방식은 함수 앞에 점(.)이 있는지 여부로 간단하게 구분할 수 있다

```jsx
var func = function (x) {
  console.log(this, x);
};

//함수
func(1); // window, 1

var obj = {
  method: func,
};

//메서드
obj.method(2); //{ method: [Function: func] } 2
```

### 화살표 함수

**ES6에서는 함수 내부에서 this가 전역객체를 바라보는 문제를 보완하고자, this를 바인딩하지 않는 화살표 함수를 도입**했다. 화살표 함수는 **실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지게 되어, 상위 스코프의 this를 그대로 활용**할 수 있다

```jsx
var obj1 = {
  outer: function () {
    console.log(this);

    var innerFunc = () => {
      console.log(this);
    };
    innerFunc(); //obj1 (상위 스코프의 this)
  },
};

obj1.outer(); //obj1
```

### **생성자 내부 함수는 새로 만들 인스턴스 자신을 가리킨다**

- 생성자 함수는 공통된 성질을 지니는 객체들을 생성하는 데 사용하는 함수로, **new** 명령어와 함께 함수를 호출하면 해당 함수가 생성자 함수로서 동작한다
- 생성자 함수로서 호출되는 경우 내부에서의 this는 곧 새로 만들 구체적인 인스턴스 자신이 된다

```jsx
var Cat = function (name, age) {
  this.bark = "야옹";
  this.name = name;
  this.age = age;
};

var choco = new Cat("초코", 7);
var nabi = new Cat("나비", 5);

console.log(choco); // Cat { bark: '야옹, name: '초코', age: 7 }
console.log(nabi); //Cat { bark: '야옹, name: '나비', age: 5 }
```

## **명시적으로 this를 바인딩**

상황에 따라 달라지는 this를 원하는 값으로 바인딩하는 방법

### **call(), apply()**

- **call** 메서드는 호출 주체인 함수를 즉시 실행하도록 하는 명령으로, **첫 번째 인자를 this로 바인딩**하고 이후의 인자들을 호출할 함수의 매개변수로 전달하면 된다.
- **apply** 메서드는 call과 동일한 역할을 수행하며, 두 번째 인자를 배열로 받는다는 점만 다르다.

```jsx
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

func(1, 2, 3); // window 1 2 3
func.call({ x: 1 }, 4, 5, 6); // {x: 1} 4 5 6
func.apply({ x: 2 }, [4, 5, 6]); // {x: 2} 4 5 6
```

### **bind()**

- **bind** 메서드는 call과 비슷하지만, 즉시 호출하지 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 하는 메서드이다.
- 함수에 this를 미리 적용할 때 혹은 부분 적용 함수를 구현할 때 활용할 수 있다.
- bind를 통해 새로 만든 함수는 **name** 프로퍼티에 **bound**라는 접두어가 붙는다는 특징이 있다. 따라서 call이나 apply보다 코드를 추적하기 수월하다.

```jsx
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
};

func(1, 2, 3, 4); //window 1 2 3 4

var bindFunc1 = func.bind({ x: 1 });
bindFunc1(5, 6, 7, 8); //{ x:1 } 5 6 7 8

var bindFunc2 = func.bind({ x: 1 }, 4, 5);
bindFunc2(6, 7); //{ x:1 } 4 5 6 7
bindFunc2(8, 9); //{ x:1 } 4 5 8 9
```

---

아래 코드를 실행하면?

```jsx
function Cat(name, age) {
  this.name = name;
  this.age = age;
}

const tabby1 = Cat("nana", 5);
console.log(tabby1.name);
```

**apply, call, bind** 등으로 **this**에 대해 주입한 상황이 아니고 **new** 키워드없이 실행한 함수 내 **this**는 **전역 객체(window)**를 바라본다.
즉 **this.name = name**의 결과는 **window.name = name**
