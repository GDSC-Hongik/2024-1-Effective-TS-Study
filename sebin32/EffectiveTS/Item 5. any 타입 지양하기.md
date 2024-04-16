# Item 5. any 타입 지양하기

TS의 타입 시스템은 점진적(gradual), 선택적(optional)

### any 타입에는 타입 안정성이 없음

 

```tsx
let age: number;
age = '12'; //error
age = '12' as any; // no error

age += 1; // runtime에서는 정상, '121'이 됨. 그러나 의도대로 동작한 것 x
```

### any는 함수 시그니처를 무시

```tsx
function calculateAge (BirthDate: Date) : number {
  //
}

let birthDate: any = '1991-01-19';
calculateAge(birthDate); // 타입 체커에서 이상 x
```

### any  타입에는 언어 서비스가 적용되지 않는다.

편집기에서는 자동 완성 기능과 적절한 도움말을 제공하는데, any는 그런 서비스가 적용되지 않음.

### any 타입은 코드 리팩토링 때 버그를 감춘다.

아이템을 선택할 수 있는 웹 어플리케이션을 만드는 상황 

```tsx
interface ComponentProps {
  onSelelctItem: (item: any) => void;
}

function renderSelector (props: ComponentProps) {/*....*/}

let selectedID : number = 0;

function handleSelectItem(item: any) {
  selectedId: item.id;
}

renderSelector({onSelelctItem: handleSelectItem});

//아래와 같이 리팩토링하는 경우 런타임에서 오류가 발생하게 됨
interface ComponentPropsd {
  onSelectItem : (id: number) => void;
}
```

### any는 타입 설계를 감춰버린다.

애플리케이션 상태와 같은 객체를 정의하는 것은 그 안의 속성 타입을 일일이 작성해야 해서 꽤 복잡하지만, 그럼에도 불구하고 any를 지양해야 하는 이유는 설계가 불분명해지기 때문이다.

### any는 타입시스템의 신뢰도를 떨어뜨린다.

사람의 실수를 바로잡기 위해 사용하는 타입 스크립트인만큼, 런타임 이전에 오류를 발견할 수 있도록, any 사용을 지양한다. 그러나 때로는 any를 써야하는 상황이 있을 수 있다.