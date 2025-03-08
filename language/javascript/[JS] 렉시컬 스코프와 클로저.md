# 렉시컬 스코프 (Lexical Scope)와 클로저 (closure)

## 렉시컬 스코프

렉시컬 스코프란 함수를 어디서 호출했는지가 아닌, 어디서 선언하였는지에 따라 결정되는 것을 말합니다. 즉, 함수가 호출되는 위치와 상관없이 함수가 선언된 위치에 따라 상위 스코프를 결정합니다.

### 렉시컬 스코프 예시

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();
bar();

// 1
// 1
```

위 코드의 결과는 `10`, `1`일 것 같지만, `1`, `1`입니다. 선언을 기준으로 봤을 때 `foo`와 `bar`의 상위 스코프가 모두 전역 스코프이기 때문입니다.

### 화살표 함수와 렉시컬 스코프

```javascript
const person = {
  name: "Kang",
  sayName: () => console.log(this.name),
};

person.sayName();
```

화살표 함수는 자신만의 `this`를 갖기 않고, 상위 스코프의 `this`를 렉시컬하게 가져옵니다. 위 코드를 기준으로 보면 `this`는 `sayName`의 호출 당시 `person`을 바인딩 하지 않고, 선언 당시의 상위 스코프인 전역 스코프(window)를 사용합니다.

## 클로저

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경(Lexical Environment)의 조합입니다.<br/>
> 즉, 클로저는 내부 함수에서 외부 함수의 범위에 대한 접근을 제공합니다<br/>
> 출처 - MDN

### 클로저 예시 코드

```javascript
function outer() {
  let a = 1;
  return function inner() {
    console.log(a);
  };
}

const closureFunc = outer();
closureFunc();

// 1
```

위 코드에서 `closureFunc`가 호출되면 `outer`가 실행된 이후 콜스택에서 제거됩니다. 즉, `outer`의 지역 변수인 `a` 또한 더 이상 유효하지 않게 되어 접근할 수 없습니다.<br/>
그러나 위 코드의 실행 결과는 변수 `a`의 값인 1입니다. 이미 콜스택에서 제거된 함수 `outer`의 변수가 어떻게 출력되었을까요?<br/><br/>
자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부 함수 밖에서 내부 함수가 호출되더라도 외부함수의 지역변수에 접근할 수 있습니다. 이런 함수를 `클로저(closure)`라고 부릅니다.<br/><br/>
다시 말해, `클로저`는 <strong>반환된 내부함수가 자신이 선언됐을 때의 환경(Lexical environment)인 스코프를 기억하여 자신이 선언됐을 때의 환경(스코프) 밖에서 호출되어도 그 환경(스코프)에 접근할 수 있는 함수</strong>를 말합니다.

### 클로저의 특징

#### 전역 변수 사용 억제 및 데이터 은닉

클로저를 이용하면 전역변수를 최소한으로 사용하고, 내부 변수를 외부에서 직접 접근할 수 없도록 해 의도치 않은 상태 변경을 방지할 수 있습니다.

```javascript
function Counter() {
  let value = 0;

  this.increase = function () {
    return ++value;
  };

  this.decrease = function () {
    return --value;
  };
}

const counter = new Counter();
console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.decrease()); // 1
```
