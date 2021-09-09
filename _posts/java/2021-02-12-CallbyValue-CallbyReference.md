---
title: Java - Call by Value vs Call by Reference
toc: true
categories:	
    - Java
tags: 
- Java
- Call by value
- Call by reference
last_modified_at:
---

   결론부터 얘기하면, `JAVA`는 항상 `call by value`이다. 다만, `primitive type` 이 아닌 `reference type` 에 한해서 `call by reference` 로 동작하는 것처럼 보일 뿐이다. 

## 자바는 call by value?

```
자바가 '레퍼런스에 의해(by reference)' 배열과 객체를 처리한다고 설명하였다.
이를 '레퍼런스에 의한 전달(pass by reference)'와 혼동하지 말기를 바란다.
'레퍼런스에 의한 전달'은 몇몇 프로그래밍 언어의 메소드 호출 규약을 설명하는데
사용되는 용어이다. '레퍼런스에 의한 전달' 언어에서는 값(심지어 기본값이라고
하더라도) 이 메소드에 바로 전달되지 않는다. 대신에, 메소드는 레퍼런스를 값으로
전달받는다. 그러므로 메소드가 매개변수를 변경한다면, 이 변경은 메소드가 리턴했을 때
반영 되어 있을 것이며, 매개변수가 기본형이라도 이는 마찬가지이다.
(여기서 기본형이란 자바의 primitive type을 말하는 거겠죠)

자바는 이렇게 하지 않는다. 자바는 '값에 의한 전달' 언어이다. 그러나 레퍼런스
타입의 경우에는 전달되는 값이 레퍼런스이다. 이것이 '레퍼런스에 의한 전달'과
똑같은 것은 아니지만 말이다. 자바가 '레퍼런스에 의한 전달'언어였다면,
레퍼런스 타입이 메소드에 전달될 때 레퍼런스에 대한 레퍼런스가 전달되었을 것이다.
```

**[참고] - 자바 인어넛셀 개정3판(오라일리 2000)**

 `JAVA` 언어로 코딩테스트를 진행할 때, **배열**이나 **객체**를 함수의 `argument`로 전달 했을 때 `call by reference`로 동작하는 것처럼 보였다. `argument`를 전달받은 메소드에서 **배열**이나 **객체**의 값을 변경하면 실제로 값이 변경되었기 때문이다.

하지만, 자바는 항상 `call by value`로 동작한다. `reference type`에 한해서 `call by reference`로 동작하듯 **보일** 뿐이다.

**예제를 통해 자세히 알아보자.**

## int 형

`int` 형 변수를 변경하는 예제이다.

```java
public class test01 {
    public static void increment(int var){

        var++;
    }
    public static void main(String[] args){
        int i=10;
        System.out.println(i);
        increment(i);
        System.out.println(i);
    }
}


[출력]
10
10
```

예상했던대로 `primitive type`인 `int`는 `call by value` 방식으로 동작해 `i`의 값이 변하지 않는다. 그저 새로운 복사 값을 `increment()` 메소드의 `argument`로 전달해주기 때문이다.

<br/>

## 객체(Object) 형

**객체**의 값을 변경시키는 예제이다.

```java
public class test02 {
    public static class MyValue{
        int value;
    }

    public static void increment(MyValue value){
        (value.value)++;
    }

    public static void main(String[] args){
        MyValue x=new MyValue();
        x.value=10;
        System.out.println(x.value);
        increment(x);
        System.out.println(x.value);

    }
}

[출력]
10
11
```

이번에는 `Myvalue`의 인스턴스인 `x`를 `argument`로 전달해줬다. 이때는 `reference` 값 자체가 전달되기 때문에 `reference`의 멤버변수를 통해 값을 변경할 수 있다.

그럼 자바는 `call by reference` 로 동작하는게 맞지 않나? 라는 생각이 들 수도 있다. 만약, 자바가 `call by reference` 였다면 레퍼런스 타입이 메소드에 전달될 때, **레퍼런스에 대한 레퍼런스**가 전달되었을 것이다. 하지만, 위 예제에서는 그저 `reference` 값 자체를 `argument`로 전달하고 있다. 즉, 자바는 항상 `call by value`로 동작한다.



<br/>

## 배열(Array) 형

**배열**의 값을 변경하는 예제이다.

```java
public class test03 {

    static void changeArr(int[] arr){

        for(int i=0;i<5;i++) arr[i]=5-i;
    }

    static void print(int[] arr){
        for(int i=0;i<arr.length;i++){
            System.out.print(arr[i]);
        }
        System.out.println();
    }

    public static void main(String[] args){
        int[] arr=new int[5];

        for(int i=0;i<5;i++){
            arr[i]=i;
        }

        print(arr);
        changeArr(arr);
        print(arr);
    }
}

[출력]
01234
54321
```

**배열**도 `reference type`이기 때문에 호출한 메소드에서 값을 변경하면 실제 배열의 값이 변경된다.

<br/>

## 문자열(String) 형

`String` 예제를 살펴보자.

```java
public class test04 {
    static void changeStr(String str,int a,char b){
        str=str.substring(0,a-1)+b+str.substring(a);
        System.out.println(str);
        
        // 새롭게 만들어진다 str은 불변
    }

    static void print(String str){
        for(int i=0;i<str.length();i++){
            System.out.print(str.charAt(i));
        }
        System.out.println();
    }

    public static void main(String[] args){
        String str1="123";
        String str2=new String("123");


        print(str1);
        changeStr(str1,1,'5');
        print(str1);

        print(str2);
        changeStr(str2,1,'5');
        print(str2);

    }
}

[출력]
123
123
123
123
```

처음에 `[출력]` 결과를 보고 도저히 이해가가지 않았다. 분명히 `String`은 `reference type`이고, **객체**, **배열**과 같이 `argument`로 메소드에 전달해줬는데 실제 값이 변경되지 않기때문이다.

이유는 바로 `String`은 `immutable`하기 때문이다. 한번 생성되면 값을 절대 변경할 수 없다.

<br/>

## 문자열(String Builder) 형

`String Builder` 예제를 살펴보자.

```java
public class test05 {

    static void changeStrBuilder(StringBuilder str,int idx,char a){
            str.setCharAt(idx,a);
    }

    public static void main(String[] args){
        StringBuilder sb=new StringBuilder("12345");

        System.out.println(sb);
        changeStrBuilder(sb,3,'7');
        System.out.println(sb);
    }
}

[출력]
12345
12375
```

`StringBuilder` 객체 `sb`는 예상대로 값이 변경된다. `StringBuilder`는 `String`과 다르게 `immutable` 하지 않다. 그렇기 때문에 `argument`로 전달된 `str`을 호출한 메소드에서 변경했을 때, 실제의 값도 변경된다.



<br/>

# Reference

- [okky - call by value vs call by reference](https://okky.kr/article/32431)