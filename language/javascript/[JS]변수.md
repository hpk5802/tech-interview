# JavaScript 변수

변수는 데이터를 임시로 저장하는 공간입니다.<br/>
변수를 만들 때 `var`, `let`, `const` 키워드를 사용하고, 셋은 변수의 선언, 할당, 범위에서 차이가 있습니다.

## 변수의 선언

```javascript
var a;
let b;
const c; // 'const' 선언을 초기화해야 합니다.
```

`var`, `let` 키워드를 사용해서 변수를 선언할 때는 초기화를 하지 않아도 됩니다. <br/>
`const`는 선언과 동시에 초기화를 해주지 않으면 에러가 발생합니다.

```javascript
var a = 1;
let b = 2;
const c = 3;

var a = 2;
let b = 3; // 블록 범위 변수 'b'을(를) 다시 선언할 수 없습니다.
const c = 4; // 블록 범위 변수 'c'을(를) 다시 선언할 수 없습니다.
```

추가로 `var`는 재선언이 가능하지만, `let`과 `const`로 선언한 변수는 재선언할 수 없습니다.<br/>
`var 키워드`를 사용해 변수를 선언하면 의도치 않게 먼저 선언된 변수 값이 재선언에 의해 변경되는 부작용이 발생합니다.

## 재할당

`let`과 `const`는 재선언 할 수 없다는 점에서 같은데요. 재할당에서 차이를 알 수 있습니다.

```javascript
var a = 1;
let b = 2;
const c = 3;

a = 2;
b = 3;
c = 4; // Uncaught TypeError: Assignment to constant variable
```

`var`과 `let`으로 선언한 변수는 값을 재할당 할 수 있지만, `const`로 선언한 변수는 재할당하면 `Assignment to constant variable` 상수에 변수를 재할당할 수 없다는 메시지를 출력합니다.

## 변수 스코프 유효범위

스코프는 변수를 참조할 수 있는 범위를 말합니다.<br/><br/>

### 함수 스코프(function-level scope)

`var`는 함수 스코프(function-level scope)로 함수 내부에 선언된 변수만 지역변수로 한정하고, 나머지는 모두 전역 변수로 취급합니다.

```javascript
var a = 1;

function func() {
  var a = 2;
  var b = 3;
  console.log(a);
  console.log(b);
}

console.log(a);
func();
console.log(b);

// 1
// 2
// 3
// ReferenceError: b is not defined
```

- `console.log(a)`는 전역변수 a를 참조해 1을 출력했습니다.
- `func()`의 `console.log(a)`는 지역변수 a를 참조해 2를 출력했습니다.
- `func()`의 `console.log(b)`는 지역변수 b를 참조해 3을 출력했습니다.
- `console.log(b)`는 b가 함수 내부에서 선언되어 함수스코프를 갖는 지역변수이기 때문에 `b is not defined`라는 에러를 출력했습니다.

### 블록 스코프(block-level scope)

`let`과 `const`는 블록 스코프(block-level scope)로 함수 내부뿐 아니라 `if`, `for`, `while` 등 코드 블록(`{}`)에서 선언된 모든 변수를 지역변수로 취급합니다.

```javascript
function loop() {
  for (var a = 0; a < 5; a++) {
    console.log(a);
  }
  console.log("for문 밖: " + a);
}
loop();
/**
 * 0
 * 1
 * 2
 * 3
 * 4
 * for문 밖: 5
 */
```

위 코드는 함수 스코프인 `var` 키워드로 a를 선언했기 때문에 for문 밖에서 a를 참조해도 `for문 밖: 5`와 같은 결과가 출력 되었습니다.<br/><br/>
let으로 동일한 코드를 작성해보겠습니다.

```javascript
function loop() {
  for (let a = 0; a < 5; a++) {
    console.log(a);
  }
  console.log("for문 밖: " + a);
}
loop();
/**
 * 0
 * 1
 * 2
 * 3
 * 4
 * ReferenceError: a is not defined
 */
```

`let`은 블록 스코프로 코드 블록 내부에서만 참조 가능한 지역변수입니다. `for`문 밖에서 참조할 수 없기 때문에 `a is not defined` 에러를 출력했습니다.

### var, let, const 요약

<table style="text-align: center">
<tr>
<td></td>
<td><strong>var</strong></td>
<td><strong>let</strong></td>
<td><strong>const</strong></td>
</tr>
<tr>
<td>재선언</td>
<td>O</td>
<td>X</td>
<td>X</td>
</tr>
<tr>
<td>재할당</td>
<td>O</td>
<td>O</td>
<td>X</td>
</tr>
<tr>
<td>유효범위</td>
<td>함수</td>
<td>블록</td>
<td>블록</td>
</tr>
</table>
