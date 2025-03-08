# this와 화살표 함수(Arrow Function)

## this

자바스크립트에서의 `this`는 현재 실행 중인 코드에서 자신이 속한 `객체` 또는 자신이 생성할 `인스턴스`를 가리키는 `자기 참조 변수`입니다. 단 this가 가리키는 값, 즉 this 의 바인딩은 <strong>함수 호출 방식에 의해 동적으로 결정</strong> 됩니다.

### 일반 함수의 this

일반 함수에서 `this`는 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 `this`에 바인딩할 객체가 동적으로 결정됩니다. 일반적으로 `this`는 브라우저에서는 `window`, `Node.js` 환경에서는 `global` 객체를 가리킵니다.<br/><br/>

여기서 함수를 호출할 때 동적으로 결정된다는 말이 무엇인지 아래 코드로 확인해보겠습니다.

```javascript
function Person(name) {
  this.name = name;
}

const person1 = new Person("Kang");
console.log(person1.name); // 'Kang' -> this = 생성된 인스턴스 (person1)

function sayName() {
  console.log(this.name);
}

const person2 = {
  name: "Kim",
  sayName: sayName,
};

person2.sayName(); // 'Kim' -> this = 호출한 객체 (person2)

const unBoundSayName = person2.sayName;
unBoundSayName(); // undefined -> unBoundSayName은 전역 함수처럼 호출 : this = 전역 객체 (window)
```

위 예시를 보면 `unBoundSayName`의 호출 결과가 `Kim`이어야 할 것 같지만, `undefined`가 출력됩니다. 이는 위에서 설명한 것 처럼 `this`가 함수가 호출되는 시점에 결정되기 때문입니다. `unBoundSayName`의 경우는 전역에서 호출했기 때문에 `undefined`가 출력된 것입니다.<br/><br/>

호출 시점에 `this`가 결정되는 내용을 더 명확하게 알 수 있는 코드를 살펴보겠습니다.

```javascript
const person = {
  name: "Kang",
  sayName: function () {
    console.log(this.name);
    function innerFunc() {
      console.log(this.name);
    }
    innerFunc();
  },
};

person.sayName();

// Kang
// undefined
*/
```

위 코드의 결과를 확인해보면 `Kang`, `undefined`가 출력되었습니다.

- `person.sayName()` : `sayName()`은 `person`이라는 객체에 의해 호출되었기 때문에 `this`가 `Kang`입니다.
- `innerFunc()` : 코드를 살펴보면 `innerFunc()`는 `.`으로 함수를 호출한 객체가 없습니다. 즉, 전역 함수처럼 동작하기 때문에 `window`를 가리키게 되는 것입니다.

### 일반 함수의 this 바인딩

일반 함수에서 `bind`, `apply`, `call` 메서드를 통해 `this`를 원하는 객체로 고정할 수 있습니다.

```javascript
// bind, apply, call 예시
function sayHello(greeting) {
  console.log(`${greeting}, ${this.name}!`);
}

const user = { name: "Kang" };

// call: 즉시 호출, 인수를 쉼표로 전달
sayHello.call(user, "Hello"); // 'Hello, Kang!'

// apply: 즉시 호출, 인수를 배열로 전달
sayHello.apply(user, ["Hi"]); // 'Hi, Kang!'

// bind: 새로운 함수를 반환, 나중에 호출 가능
const boundSayHello = sayHello.bind(user, "Hey");
boundSayHello(); // 'Hey, Kang!'
```

## 화살표 함수

화살표 함수는 `es6`에서 도입된 방식으로, 기존 함수 선언 방식을 좀 더 간결하게 처리하고 가독성 높은 코드를 작성할 수 있게 합니다.

### 화살표 함수 사용법 및 특징

```javascript
const arrowFunc = () => console.log("Arrow Function");
arrowFunc(); // "Arrow Function"
```

- 화살표 함수는 `function` 키워드를 사용하지 않고 `=>`를 사용해 정의합니다.
- 화살표 함수는 익명 함수로, 호출하기 위해서는 함수 표현식을 사용합니다.
- 함수 구문이 한 줄이라면 위 예시 처럼, 암묵적으로 `{}` 과 `return`을 생략할 수 있습니다.
- 함수 구문이 여러줄이라면 `{}`과 `return`을 명시해야 합니다.
- 화살표 함수는 생성자 함수로 사용할 수 없고, `new 키워드`와 함께 사용할 수 없다.
- `arguments` 객체를 갖지 않고, 필요하다면 `나머지 매개변수(rest parameters)`를 사용할 수 있다.
- 일반 함수와 다르게 `this`가 상위 스코프의 `this`를 가리킨다.

### 화살표 함수의 this

화살표 함수의 `this`는 상위 스코프의 `this`를 가리킵니다. 코드로 살펴보겠습니다.

```javascript
const person = {
  name: "Kang",
  sayName: function () {
    cnosole.log(this.name);
    const innerFunc = () => {
      console.log(this.name);
    };
    innerFunc();
  },
};

person.sayName();

// Kang
// Kang
```

일반 함수의 `innerFunc()` 호출 결과인 `undefined`와 다르게 `Kang`이 출력되었습니다. 이는 `innerFunc`가 화살표 함수이기 때문에 `this`가 상위 스코프인 `sayName`의 `this`를 가리키기 때문입니다.

### 화살표 함수 this 주의사항

화살표 함수의 `this`는 상위 스코프의 `this`를 가리킵니다. 그럼 아래 코드는 어떻게 동작할까요?

```javascript
const person = {
  name: "Kang",
  sayName: () => console.log(this.name),
};

person.sayName();

// undefined
```

결과는 `undefined`가 출력됩니다. 화살표 함수부터 위로 올라가보면, `sayName`이 정의된 상위 스코프는 `전역 스코프`입니다. 즉, `this`가 `window`를 가리키는 것입니다.<br/><br/>
위 예시처럼, 메서드나 생성자 함수에 화살표 함수를 사용하면 의도한 대로 this가 바인딩되지 않을 수 있어 주의 해야합니다.
