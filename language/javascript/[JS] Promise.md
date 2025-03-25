# Promise와 async/await

JavaScript에서 비동기 프로그래밍을 처리하는 두 가지 주요 방법은 **Promise**와 **async/await**입니다. 이 두 가지 방법은 비동기 작업을 보다 쉽게 관리하고, 코드의 가독성을 높이는 데 도움을 줍니다.

## 1. Promise

**Promise**는 비동기 작업의 완료 또는 실패를 나타내는 객체입니다. 값이 아닌 **미래에 값이 반환 될 것이라는 약속**을 담은 객체로 `.then()`과 `.catch()`를 사용해 완료 후 값을 사용할 수 있습니다.<br/>
Promise는 세 가지 상태를 가질 수 있습니다:

- **Pending**: 초기 상태, 작업이 아직 완료되지 않음.
- **Fulfilled**: 작업이 성공적으로 완료됨.
- **Rejected**: 작업이 실패함.

### 예시 코드

```javascript
// Promise 생성
const promise = new Promise((resolve, reject) => {
  const success = true; // 비동기 작업의 성공 여부

  if (success) {
    resolve("성공");
  } else {
    reject("실패");
  }
});

// Promise 사용
promise
  .then((result) => {
    console.log(result); // "성공"
  })
  .catch((error) => {
    console.error(error); // "실패"
  });
```

### 설명

- `Promise` 객체를 생성하고, 비동기 작업의 성공 여부에 따라 `resolve` 또는 `reject`를 호출합니다.
- `then()` 메서드는 Promise가 `fulfilled` 상태일 때 실행되며, `catch()` 메서드는 `rejected` 상태일 때 실행됩니다.

## 2. async/await

**async/await**는 Promise를 기반으로 한 비동기 프로그래밍의 문법입니다. `async` 키워드를 사용하여 비동기 함수를 정의하고, `await` 키워드를 사용하여 Promise가 해결될 때까지 기다립니다.

### 예시 코드

```javascript
// 비동기 함수 정의
async function fetchData() {
  try {
    const response = await promise; // Promise가 해결될 때까지 기다림
    console.log(response); // "성공"
  } catch (error) {
    console.error(error); // "실패"
  }
}

// 함수 호출
fetchData();
```

- `async` 키워드가 붙은 함수는 항상 Promise를 반환합니다.
- `await` 키워드는 Promise가 해결될 때까지 코드 실행을 일시 중지합니다. 이로 인해 비동기 작업을 보다 직관적으로 처리할 수 있습니다.

## 결론

- **Promise**는 비동기 작업의 결과를 나타내는 객체로, `then()`과 `catch()` 메서드를 사용하여 결과를 처리합니다.
- **async/await**는 Promise를 보다 간결하고 직관적으로 사용할 수 있게 해주는 문법으로, 비동기 코드를 동기 코드처럼 작성할 수 있습니다.

### 참고 출처

- [async/await 문법 - toss payments 블로그](https://docs.tosspayments.com/blog/async-await-example)
- [MDN Web Docs - Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN Web Docs - async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN Web Docs - await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
