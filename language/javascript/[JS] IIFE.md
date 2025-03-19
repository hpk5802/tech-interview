# IIFE (Immediately-Invoked Function Expression)

## 정의

`IIFE`는 정의되자마자 즉시 실행되는 함수입니다. 일반적으로 자바스크립트에서 클로저를 생성하고, 변수의 스코프를 제한하는 데 사용됩니다.

## 문법

IIFE는 다음과 같은 형식으로 작성됩니다:

```javascript
(function () {
  // 코드 블록
})();
```

또는 화살표 함수를 사용할 수도 있습니다:

```javascript
(() => {
  // 코드 블록
})();
```

## 사용 목적

1. **스코프 보호**: `IIFE`를 사용하면 변수의 스코프를 제한하여 전역 네임스페이스 오염을 방지할 수 있습니다.
2. **클로저 생성**: `IIFE`는 클로저를 생성하여 내부 상태를 유지할 수 있습니다.
3. **모듈화**: `IIFE`는 모듈 패턴을 구현하는 데 유용합니다.

## 예제 - IIFE

```javascript
let counter = (function () {
  let count = 0;
  return function () {
    return ++count;
  };
})();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

위 예시에서 `count`는 `IIFE` 내부에서 선언되므로 지역 스코프를 가집니다. `IIFE`가 실행된 후에는 `count`가 콜 스택에서 제거되어 외부에서 접근이 불가능합니다.<br/><br/>
그러나 `클로저`가 선언 될 당시 렉시컬 스코프를 기억하기 때문에, `IIFE`가 실행된 후에도 `count`에 접근 할 수 있어 `클로저`가 호출될 때마다 `count`가 증가합니다.

## 예제 - IIFE 사용❌

```javascript
const counter = function () {
  let count = 0;
  return function () {
    return ++count;
  };
};

const iife1 = counter(); // 전역 스코프에 iife1 추가
console.log(iife1()); // 1
console.log(iife1()); // 2
console.log(iife1()); // 3
```

위 예시는 `IIFE`를 사용하지 않은 경우입니다.<br/>
변수 `count`에 접근하기 위해서 `iife1`를 전역 스코프에 추가했습니다. 해당 변수는 프로그램 전체 수명 동안 메모리에 남아 있게 되어, 메모리 낭비가 발생할 수 있습니다. 또한 변수 이름 충돌에 의해 예기치 못한 동작을 초래할 수 있습니다.

## 장점

- **변수 은닉**: 외부에서 접근할 수 없는 변수를 만들 수 있습니다.
- **즉시 실행**: 코드가 정의되자마자 실행되므로 초기화 작업을 간편하게 처리할 수 있습니다.

## 단점

- **디버깅 어려움**: IIFE 내부의 변수는 외부에서 접근할 수 없기 때문에 디버깅이 어려울 수 있습니다.
- **가독성 저하**: 복잡한 IIFE는 코드의 가독성을 떨어뜨릴 수 있습니다.

## 결론

IIFE는 자바스크립트에서 유용한 패턴으로, 스코프를 관리하고 클로저를 생성하는 데 효과적입니다. 그러나 사용 시 가독성과 디버깅의 어려움을 고려해야 합니다.

## 참고 출처

- [MDN Web Docs - Immediately Invoked Function Expression (IIFE)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)
- [JavaScript.info - IIFE](https://javascript.info/closure#iife)
