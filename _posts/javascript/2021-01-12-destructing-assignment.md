---
title: 비구조화 할당 [JAVASCRIPT]
toc: true
categories:	
    - Javascript
tags:
- destructing assignment
- javascript
last_modified_at: 
---

 자바스크립트 **비구조화 할당**에 대해 알아보자.

**비구조화 할당(Destructing Assignment)** 이란 배열 또는 객체에서 데이터를 별개 변수로 추출할 수 있게하는 **Javascript** 표현식이다.  말그대로, 구조화 되지 않은 형식으로 데이터를 할당하고, 추출하는 것이라고 할 수 있다. 예제를 통해 실습해보자. [온라인 테스트 site](https://jsbin.com/nuzatunoru/edit?js,console,output)에서 **Test** 할 수 있다.

# 배열

### 기본 할당

배열에서 기본할당은 다음과 같다.

```javascript
let foo = ["one","two","three"];

let [on,tw,th]=foo;

console.log(on);
console.log(tw);
console.log(th);

[출력]
"one"
"two"
"three"
```

### 변수 교환

이런 방식으로도 할당이 가능하다. 형식에 자유로운 느낌이 강하다.

````javascript
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
````



### ... 구문(전개 연산자)

`...` 구문은 배열이나 객체 데이터를 별개로 분류하고자 할 때, 사용한다. **rest** 변수에는 a, b에 할당되지 않는 나머지 배열 원소들이 할당된다.

```javascript
let a,b,rest;
[a,b] = [1,2];
console.log(a); // 1
console.log(b); // 2

[a,b,...rest]=[1,2,3,4,5];
console.log(a); // 1
console.log(b); // 2
console.log(rest); // [3,4,5]
```

### ... 구문, 2차원 배열

마찬가지로 2차원 배열에서도 가능하다. `...` 구문은 유용하게 사용되므로, 익혀두자.

```javascript
let a;
a=[[1,2,3],[4,5,6],[7,8,9]];
console.log(...a);

[출력]
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
```

 `...` 구문을 2차원 배열에 대해서 실행한 예시이다. 편리하게 사용 가능한 장점이 있다.

```javascript
const a=[[1,2,3],[4,5,6],[7,8,9]];

// 구문 error 방지를 위해 Math.max로 감싸줌
Math.max(...a.map(b=>{
    console.log(b);
	b.forEach(o=>{
      console.log(o);
    })
}));

[출력]
[1, 2, 3]
1
2
3
[4, 5, 6]
4
5
6
[7, 8, 9]
7
8
9
```

### ... 구문, 깊은복사(Deep Copy)

`=` 은 얕은 복사된다. 즉, 같은 `reference`를 갖기 때문에 `arr`의 원소가 바뀌면 `copy`의 원소도 변경된다. 그에 반해 `...` 구문(전개 연산자)를 사용하면, **deep copy** 가 가능하다. 새롭게 배열을 생성해 할당하기 때문이다.

```javascript
let arr = [1,2,3];
let copy = arr;
let [...deepCopy1] = arr;
let deepCopy2 = [...arr];

arr[0] = "shallowCopy";
console.log(arr);
console.log(copy);
console.log(deepCopy1);
console.log(deepCopy2);

[출력]
["shallowCopy", 2, 3]
["shallowCopy", 2, 3]
[1, 2, 3]
[1, 2, 3]
```



### 정규식

 정규식에서도 유용하게 사용된다. 정규식을 통해 `URL`을 파싱하고, 해당하는 부분을 알맞는 키워드에 할당하고 있다.

```javascript
var url = "https://developer.mozilla.org/en-US/Web/JavaScript";

var parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
console.log(parsedURL); 

var [, protocol, fullhost, fullpath] = parsedURL;

console.log(protocol); 
console.log(fullhost);
console.log(fullpath);

[출력]
["https://developer.mozilla.org/en-US/Web/JavaScript", 
 "https", "developer.mozilla.org", "en-US/Web/JavaScript"]
"https"
"developer.mozilla.org"
"en-US/Web/JavaScript"
```





## 객체

배열과 같은 형식으로 객체에서도 비구조화 할당이 가능하다.

```javascript
({a,b}={a:1,b:2});
console.log(a); // 1
console.log(b); // 2

({a,b,...rest}={a:1,b:2,c:3,d:4});
console.log(a); // 1
console.log(b); // 2
console.log(rest); // [object Object] {c: 3,d: 4}
```



### 기본 할당

객체에 객체를 할당할 수 있다.

````javascript
let a = { b: 22 , c: 32};
let {b,c} = a;

console.log(b); // 22
console.log(c); // 32
````

### 선언 없는 할당

`(   )` 는 선언 없이 객체에 비구조화 할당을 할 때, 필요한 구문이다. 여기서 괄호 없는 `{a,b}={a:1,b:2}`는 유효한 구문이 아님을 기억하자.

```javascript
({a,b}={a:1,b:2});
console.log(a);  // 1
console.log(b);  // 2
```

### 중첩 객체

복잡한 구조를 갖는 중첩 객체도 비구조화 할당이 가능하다.

```javascript
let metadata = {
    title: "guen",
    translations: [
       {
        locale: "de",
        localization_tags: [ ],
        last_edit: "2021-01-12T09:38:37",
        url: "/de/docs/Tools/Scratchpad",
        title: "JavaScript-Studying"
       }
    ],
    url: "/UTF-8/docs/Tools/Scratchpad"
};

let { 
     title: englishTitle, 
     translations: [{ title: localeTitle }] 
    } = metadata;

console.log(englishTitle); 
console.log(localeTitle);  

[출력]
"guen"
"JavaScript-Studying"
```

### 중첩 객체 - for of 구문

`for of` 구문을 돌며, 객체를 출력할 수 있다. 다양한 곳에 활용가능해보인다.

```javascript
let FootballTeam = [
  {
    captain: "황선홍",
    member: {
      first: "유상철",
      second: "홍명보",
      third: "김태영"
    },
    age: 35
  },
  {
    captain: "박지성",
    member: {
      first: "이천수",
      second: "기성용",
      third: "손흥민"
    },
    age: 25
  }
];

for (let {captain: n, member: { first: f } } of FootballTeam) {
  console.log("Captain: " + n + ", first: " + f);
}
```



<br/>

# Reference

- [Mozilla - 비구조화 할당](https://developer.mozilla.org.cach3.com/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

