---
title: Javascript 기술 면접 정리
toc: true
categories:	
    - Interview
tags:
- Javascript
last_modified_at: 
---



`Daily Update`

## javascript

### undefined vs null

- `undefined` : 어떠한 값으로도 할당되지 않아 자료형이 정해지지 않은(undefined) 상태
- `null` : null로 값이 할당된 상태. 즉 null은 자료형이 정해진(defined) 상태, 어떤 값이 의도적으로 비어있음을 표현

### var vs let vs const

- `var` : function-scoped, 재선언 가능, 재할당 가능
- `let` : block-scoped, 재선언 불가능, 재할당 가능
- `const` : block-scoped, immutable , 재선언 불가능, 재할당 불가능

### scope

- `scope` : 허용범위, 코드에 접근할 수 있는 범위
- `global-scope` : 전역변수 scope, 최상위 scope
- `local-scope` : 지역변수 scope
- 전역 스코프안에 있는 하위 스코프를 지역스코프라고 한다.
- 하위 스코프는 상위 스코프를 참조할 수 있다.
- 상위 스코프는 하위 스코프를 참조할 수 없다.

#### Ref.

[스코프란?](https://velog.io/@mgm-dev/%EC%8A%A4%EC%BD%94%ED%94%84%EB%9E%80-%EB%AD%98%EA%B9%8C)

### hoisting

- 함수의 선언부가 해당하는 `scope`영역의 최상단으로 끌어올려지는 것
- `var` 은 **function-scoped** 이므로 함수의 최상단으로 선언부(var value)가 **호이스팅**된다.

```javascript
fucntion hoistingEx(){
    // 이 위치로 hoisting 된다.
	console.log("value="+value);
	var value = 10;
	console.log("value="+value);
}
hoistingEx();

[실행결과]
value=undefined
value=10
```

### this (in javascript)

- `this` 키워드는 기본적으로 전역객체이다. 브라우저에서는 `window`가 된다.

```javascript
console.log(this);
console.log(this===window);

[출력]
[object Window]
true
```

- `this` 를 생성자 혹은 객체의 메소드로 사용하지 않는경우 `this`는 전역객체가 된다.

```javascript
const person=(name,gender)=>{
  this.name=name;
  this.gender=gender;
}

let gwang=person("광근","남");
console.log(gwang); // undefined
console.log(name);  // 광근
console.log(gender); // 남
```

이때, `gwang`은 `person`함수가 아무것도 `return`하지 않기 때문에 어떤 값으로도 초기화되어있지 않다. 따라서 `undefined`가 발생한다.

- `this`는 해당 메서드를 호출한 객체로 바인딩된다.

```javascript
var myobject={
    name:'foo',
    sayName:function(){
        console.log(this.name);
    }
};

var otherobject={
    name:'bar'
};

otherobject.sayName=myobject.sayName;

myobject.sayName();
otherobject.sayName();

[결과]
foo
bar
```

#### Ref.

[javascript-this](https://hyunseob.github.io/2016/03/10/javascript-this/)

### closure

- 클로저는 내부함수가 외부함수의 context에 접근할 수 있는 것을 가리킨다.
- 내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나서 **외부함수가 소멸된 이후**에도 **내부함수가 외부함수의 변수에 접근** 할 수 있다. 이러한 메커니즘을 클로저라고 한다.

```javascript
function makeAdder(x) {
  var y = 1;
  return function(z) {
    y=100;
    return x + y + z;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

- `javascript`에서 우리가 쓰는 많은 코드는 이벤트 기반이다. `click`, `key function` 등
- 이러한 경우 코드는 콜백형태로 전달된다. => 콜백 : 이벤트에 응답하여 실행되는 단일함수
- 위와 같은 상황에서 `closure`를 활용할 수 있다.
- 아래는 사용자의 `click`에 따라 `font-size`를 변경하는 기능의 자바스크립트코드이다.

```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```

#### Ref.

[MDN - Closure](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

[생활코딩 - Closure](https://opentutorials.org/course/743/6544)

### JS Engine

![jsengine](https://user-images.githubusercontent.com/49560745/108459270-aa808e00-72b9-11eb-9ce1-1278bb7151d7.png)

#### js Engine

- 자바스크립트 엔진은 **Memory Heap** 과 **Call Stack**으로 이루어져있다.
- **Memory Heap** : 메모리 할당이 일어나는 곳
- **Call Stack** : 코드가 실행될 때 코드가 쌓이는 곳. 선입후출구조
- 자바스크립트는 **단일 스레드** 프로그래밍언어이다. 즉, **Call Stack**이 하나이다.

#### Web API

- 브라우저에서 제공하는 **API**
- DOM, Ajax, Timeout 등이 있다.
- **Call Stack**에서 실행된 **비동기 함수**는 **Web API**를 호출하고, **Web API**는 콜백함수를 **Callback Queue**에 밀어넣는다.

#### Callback Queue

- 비동기적으로 실행되는 콜백함수가 보관되는 곳이다. 선입선출구조

- **Event Listner**나 **setTimeout** 이후 실행되는 함수 등이 저장된다.

#### Event Loop

- 이벤트루프는 **Call Stack**과 **Callback Queue**의 상태를 점검하고, **Call Stack**이 빈상태가 되었을 때 **Callback Queue**의 첫번째 콜백을 **Call Stack**에 밀어넣는다.

<br/>

자바스크립트는 단일 스레드 프로그래밍언이이지만 **Web API**, **Callback Queue**, **Event Loop**를 활용해 비동기적인 처리를 할 수 있다.

### Call back

- 함수가 실행되고 난 후에 특정한 함수를 실행하는 것

### 실행 컨텍스트(Execution Context)

- 코드들이 실행되기 위한 환경

### 원시 타입 vs 참조 타입

- `javascript`에서  원시 값이란 **객체**가 아니면서 **메서드**도 가지지 않는 데이터이다.
- 원시 값은 `string`,`number`,`bigint`,`boolean`,`undefined`,`symbol` 6가지 타입이있다.