---
title: Java - String vs StringBuilder vs StringBuffer
toc: true
categories:	
    - Java
tags: 
- Java
- String
last_modified_at:
---

 **String**, **StringBuilder**, **StringBuffer** 는 PS(Problem Solving)에서 **Stirng** 객체를 컨트롤 할때 한 번씩 들어봤을 것이다. 막연하게 속도의 차이가 있다는 점만 알고 있었기에, 이번 포스팅에서는 **String**, **StringBuilder**, **StringBuffer** 특징을 간략하게 알아보려한다.

먼저 요약하자면, **Stirng** 은 **immutable(불변)**, **StringBuilder**, **StringBuffer**는 **mutable(가변)** 이라는 점을 기억하자.

# String

**String** 객체는 한번 생성되면 할당 된 메모리 공간이 변하지 않는다. `+` 연산자나 `concat` 메소드를 이용해 기존 **Stirng** 객체 문자열에 새로운 문자열을 붙일 수 있다. 이때, **String** 객체는 기존 문자열에 새로운 문자열을 붙이는 것이 아니라 **새로운 Stirng 객체가 다른 메모리 공간에 적재되는 것이다.** 즉, 기존 **String** 객체는 **immutable(불변)** 한다.

예제로 살펴보자. 자바의 **hashCode()** 메소드는 각 객체의 주소값을 변환하여 생성한 객체의 고유한 정수값이다. 즉, 두 객체가 동일한지 파악할 때 사용한다.

```java
String str = "ABCD";
String newStr=str+"E";
        
System.out.println("str: " + str.hashCode());
System.out.println("newStr: " + newStr.hashCode());

newStr=str;
System.out.println("newStr: " + newStr.hashCode());
newStr=newStr.concat("E");
System.out.println("newStr: " + newStr.hashCode());

[결과]
str: 2001986
newStr: 62061635
newStr: 2001986
newStr: 62061635
```

 `+` 연산이나 `conact` 을 사용한 경우, 기존의 **String** 객체의 공간이 아닌 새로운 공간에 새로운 문자열을 할당한다. 

![stringconcat+](https://user-images.githubusercontent.com/49560745/104872024-09d44100-5990-11eb-8d91-072a2c5554db.png)





# StringBuilder

**StringBuilder** 는 **String** 객체와는 다르게 기존 버퍼의 크기(메모리 공간)를 늘리며 유연하게 작동한다. 

아래의 예제를 보자.

````java
StringBuilder str = new StringBuilder();

str.append("TEST1");
System.out.println("str: " + str.hashCode());

str.append("ISEQUAL");
System.out.println("str: " + str.hashCode());

[결과]
str: 2001112025
str: 2001112025
````

**hashcode**가 동일하다. 즉, 동일한 메모리 공간의 버퍼 크기를 키워 할당한 것을 알 수 있다.

![StringBuilder](https://user-images.githubusercontent.com/49560745/104871805-6b47e000-598f-11eb-9e70-f9d8c8cb91ba.png)

# StringBuffer

**StringBuffer**도 마찬가지로 **String** 객체와는 다르게 기존 버퍼의 크기(메모리 공간)를 늘리며 유연하게 작동한다. 

아래의 예제를 보자.

```java
StringBuffer str = new StringBuffer();

str.append("TEST1");
System.out.println("str: " + str.hashCode());

str.append("ISEQUAL");
System.out.println("str: " + str.hashCode());

[결과]
str: 2001112025
str: 2001112025
```

**StringBuilder** 와 마찬가지로 **hashcode**가 동일하다. 즉, 동일한 메모리 공간의 버퍼 크기를 키워 할당한 것을 알 수 있다.

![StringBuffer](https://user-images.githubusercontent.com/49560745/104871762-4489a980-598f-11eb-925e-4bc2a892b8e6.png)

 지금까지 내용을 요약해보면 다음과 같다.

- String : **immutable(불변)**, **새로운 메모리 공간에 할당**
- StringBuilder, StringBuffer : **mutable(가변)**, **기존 메모리 공간에 추가**

<br/>

# StringBuilder vs StringBuffer

그렇다면 **StringBuilder** 와 **StringBuffer** 간의 차이는 없는 것일까? 그렇지 않다. **StringBuilder**는 **ansynchronization(비동기화)** 클래스로서 동기화를 보장하지 않고, **StringBuffer**는 **synchronization(동기화)** 를 보장한다.

![StringBuilder](https://user-images.githubusercontent.com/49560745/104870367-b2cc6d00-598b-11eb-9c39-39907ba9f7d4.png)

**StringBuilder** 는 동기화를 보장하지 않기 때문에 단일 쓰레드 환경에서 사용하는게 안전하다.

![StringBuffer](https://user-images.githubusercontent.com/49560745/104870927-15723880-598d-11eb-93d2-f969b4c1ddda.png)

**StringBuffer** 는 동기화를 보장하기 때문에 멀티 쓰레드 환경에서 사용해도 안전하다.

# 결론

| String          | StringBuilder               | StringBuffer            |
| --------------- | --------------------------- | ----------------------- |
| immutable(불변) | mutable(가변)               | mutable(가변)           |
|                 | asynchronization(비동기 화) | synchronization(동기화) |

- **StringBuilder** 와 **StringBuffer** 는 `+` 나 `concat` 이 새로운 메모리 공간을 할당해 새로운 **String** 객체를 생성하는 것과 달리 기존 **String** 객체에 연산을 하기 때문에, 연산 속도가 보다 빠르다.

- **StringBuilder** 는 동기화를 보장하지 않고, **StringBuffer** 는 동기화를 보장한다. 따라서, 멀티 쓰레드 환경에서는 **StringBuffer**를 사용하는게 보다 안전하다.

<br/>

# Reference

- [[길은 가면 뒤에 있다] String, StringBuilder,StringBuffer]( https://12bme.tistory.com/42)

- [Java8 API, docs](https://docs.oracle.com/javase/8/docs/api/)

- [[KH BYUN] StrinbBuilder,StringBuffer](https://novemberde.github.io/2017/04/15/String_0.html)