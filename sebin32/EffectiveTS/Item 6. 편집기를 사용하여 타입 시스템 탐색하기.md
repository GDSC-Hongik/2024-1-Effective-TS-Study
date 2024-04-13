# Item 6. 편집기를 사용하여 타입 시스템 탐색하기

편집기를 통해서 사용할 수 있는 언어 서비스 

- 코드 자동 완성
- 명세 (specification)
- 검색
- 리팩토링

예시 > 

```tsx
function getElement(elOrID :string|HTMLElement|null) :HTMLElement {
  if (typeof elOrID === 'object') {
    return elOrID;  
    // null이거나 HTMLElement일 수 있기 때문에 HTMLElement에 할당할 수 없음
  } else if (elOrID === null) {
    return document.body;
  } else {
    const el = document.getElementById(elOrID);
    return el;  
    // HTMLElement | null 형식은 HTMLElement 형식에 할당할 수 없음
  }
}
```

lib.dom.d.ts가 DOM의 타입 선언 파일