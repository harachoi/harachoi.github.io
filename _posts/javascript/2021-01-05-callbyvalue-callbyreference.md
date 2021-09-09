---
title: Call by Value vs Call by Reference [JAVASCRIPT]
toc: true
categories:	
    - Javascript
tags:
- call by value
- call by reference
- javascript
last_modified_at: 
---

**call by value**, **call by reference**의 개념과 자바스크립트 내에서는 어떻게 사용되는지 알아보자.

<br/>



 사실 이 포스팅을 작성하게 된 계기는 [프로그래머스 자물쇠와 열쇠 ](https://programmers.co.kr/learn/courses/30/lessons/60059) 이 문제를 풀이하다 '맞왜틀'에 빠졌기 때문이다. 배열 **argument**를 함수의 매개변수로 넘겨주며, 기본 배열 값의 변경되어 문제가 발생했다. 개념의 중요성을 다시 한번 느끼게 된다.

시작하기에 앞서, **parameter** 와 **argument**의 정의를 정리해봤다. **Call by value**, **Call by Reference**를 이해하기 위해 참고하면 좋을 것같다.

* [참고] parameter vs argument

```javascript
parameter는 함수 혹은 메서드내에서 정의되는 변수명이다.
argument는 함수 혹은 메서드를 호출할 때 전달하거나 입력되는 실제 값이다.

ex)

function test(para1,para2){
	return para1 + " " + para2;
}

// test 함수 내에 정의된 para1, para2는 parameter이다.

test(argu1,argu2);
// test 함수를 호출할 때 전달되는 argu1, argu2는 argument이다.

```

<br/>

# Call by value, Call by Reference란 무엇인가?

본격적으로 **argument**를 함수 혹은 메소드의 매개변수로 전달하는 **두 방식**에 대해 알아보자.

```
Call by value - 값을 전달한다.
Call by Refrence - 참조 값을 전달한다.
```





<br/>

# Call by Value

call by value의 특징은 이렇다.

- **argument**가 값으로 넘어온다.

- 값이 넘어올 때, 값을 **복사**하여 넘겨준다.
- 호출하는 곳에서 값을 **복사**하여 넘겨주기 때문에, 호출 당한 곳에서 호출자의 argument 값을 **변경**할 수 없다. 즉, 원본 **argument** 값이 **변경 될 위험이 없다.** 
- **Call by value 타입**에는 `number`, `string`, `boolean` 등이 있다.

````javascript
let callee="피 호출자";

function TryChangeCallee(parameter){
    parameter="호출자";
}

TryChangeCallee(callee); // 호출자

console.log(callee); // 출력 : 피 호출자
````

 **Call by value** 타입인 `string` "피 호출자"를 함수의 parameter로 넘겨줬다. TryChangeCallee 함수에서 변수 callee의 값이 변경되지 않는 것을 확인할 수 있다.

<br/>

# Call by Reference

call by reference의 특징은 이렇다.

- **argument**로 **reference**를 넘겨준다.
  - reference는 메모리 주소를 담고있는 혹은 가리키는 변수이다.
- **reference** 자체를 넘기기 때문에 **call by value와 다르게** 값을 복사해 넘기는 형태가 아니다.
- 호출하는 곳에서 참조 값을 넘겨주기 때문에, 호출 당한 곳에서 호출자의 argument 값을 **변경**할 수 있다. 즉, 원본 **argument** 값이 **변경 될 위험이 발생한다.**
- **Call by Reference** 타입에는 `array`, `object`, `date` 등이 있다.

```javascript
let callee=[1,2,3,4];

function TryChangeCallee(parameter){
    parameter[0]=0;
}

TryChangeCallee(callee); // 호출자

console.log(callee); // 출력 : [0,2,3,4];
```

이번에는 **call by reference** 타입인 `array`를 함수의 parameter로 넘겨주었다. 아래 그림과 같다.

![image](https://user-images.githubusercontent.com/49560745/103621105-0bf1d500-4f78-11eb-9c95-c87288950eff.png)

그리고 `parameter[0]=0` 으로 값을 변경해줬다. 출력 결과는 `[0,2,3,4]`로 원본 배열인 callee의 0번째 `index`값이 변경되었음을 확인할 수 있다.



![image](https://user-images.githubusercontent.com/49560745/103621441-8f132b00-4f78-11eb-877e-4127a58024ea.png)

이러한 원리로 **callee**와 같은 값을 참조하고 있는 **parameter**에서 특정 값이 변경되면 **callee**의 값도 변경되는 것이다.



- `0x001`,`0x002`, `0x003`은 임의로 메모리주소를 지정한 것이다.
- **메모리주소 - 메모리 위치에 대한 식별자, 메모리가 존재하는 공간**

<br/>

# Refernce 원본을 유지하는 방법

그렇다면 **call by reference**로 값을 넘겨줄 때, 어떻게 원본 **reference** 값이 변경되지 않고 **parameter**로 넘어온 **reference**를 사용할 수 있을까?

답은 **새로운 객체를 생성하고, 매개변수 값을 이 새로운 객체에 할당하는 것**이다.

```javascript

let callee=[1,2,3,4];
function TryChangeCallee(parameter){
    let newarr=Array.from(Array(4));
    /*
       원소 하나하나를 복사해줘야 Deep copy(깊은 복사)가 된다.
       => 새로운 메모리가 할당되고, 복사된다.
       newarr=parameter는 shallow copy(얕은 복사)가 된다.
       =>  참조값에 할당된다.
    */
   
    for(let i=0;i<parameter.length;i++){
        newarr[i]=parameter[i];
    }
    newarr[0]=0;
}

TryChangeCallee(callee); // 호출자

console.log(callee); // 출력 : [1,2,3,4];
```

![image](https://user-images.githubusercontent.com/49560745/103623243-2f6a4f00-4f7b-11eb-8fb3-2ea40e942f70.png)

자바스크립트는 항상 **call by value** 방식만 존재한다고 한다. 결국, 변수가 가리키는 메모리에 적재되어 있는 값을 복사해 전달하기 때문이다. 

- call by value : 값을 복사 후 전달
- call by refernce : refernce를 복사 후 전달

<br/>

# Reference

- [jimmyjoo.log - call by value vs call by reference](https://velog.io/@jimmyjoo/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%8F%89%EA%B0%80%EC%A0%84%EB%9E%B5-Call-By-Value-vs-Call-By-Reference-vs-Call-By-Sharing)
- [bbackteaho - 얕은복사, 깊은복사](https://bbaktaeho-95.tistory.com/37)

