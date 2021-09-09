---
title: Spring boot기반 Web Application 개발[3] - View 설정하기
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

Spring boot를 활용해 view 를 구현해보자. 

우선, 도메인 접속 시 기본 페이지가 되는 index.html 을 구현하자. 

# index.html 구현

````
파일명 : index.html
위치 :  resources/static/index.html
````

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
Hello
<a href="/hello">hello</a>
</body>
</html
```

다시 서버를 실행해, locallhost:8000에 접속하면 **hello**를 포함한 페이지가 로드된다. 파일 위치에 주목하자. 정적인 화면은 static 폴더 밑에 구현한다.

![image](https://user-images.githubusercontent.com/49560745/103200313-58159780-4930-11eb-9e5e-613ffdd6f5f0.png)

# 동적 페이지 구현

이제, **Controller**를 활용해 동적인 페이지를 만들어보자.

실행 순서는 다음과 같다. 

```
1) 요청 - locallhost:8000/hello
2) 내장 톰캣 서버에 전달
3) 'hello'와 매핑되는 컨트롤러 메서드 찾기
4) 메서드 실행 => model(data:hello) 반환
5) viewResolver는 model을 html로 변환 후 브라우저에 반환
```



![스프링부트 동적페이지 동작원리](https://user-images.githubusercontent.com/49560745/103200051-bbeb9080-492f-11eb-96b4-3df56e587d63.png)

이를 코드로 구현해보자.

 ```
파일명 : HelloController.java
위치 : java/hello.hellospring/controller/HelloController.java
 ```

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String Hello(Model model){
        model.addAttribute("data","hello!!");
        return "hello";
    }
}
```



```
파일명 : hello.html
위치 : resources/templates/hello.html
```

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```

이제, locallhost:8000/hello에 접속하면 해당 화면이 반환 됨을 확인할 수 있다. ${data}는 컨트롤러에서 반환 된 **hello!!**를 받는다.

![스피링부트 view 페이지 로드](https://user-images.githubusercontent.com/49560745/103200469-bfcbe280-4930-11eb-8865-baf5e1c2a571.png)

## 참고

스프링 부트가 제공하는 Welcome Page 기능

```
- static/index.html 을 올려두면 Welcome page 기능을 제공한다.
- https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-
bootfeatures.html#boot-features-spring-mvc-welcome-page

```

thymeleaf 템플릿 엔진

```
- thymeleaf 공식 사이트: https://www.thymeleaf.org/
- 스프링 공식 튜토리얼: https://spring.io/guides/gs/serving-web-content/
- 스프링부트 메뉴얼: https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/
html/spring-boot-features.html#boot-features-spring-mvc-template-engines
```

spring-boot-devtools 라이브러리

```
해당 라이브러리를 추가하면, html 파일을 컴파일만 해주면 서버 재시작 없이
view 파일 변경이 가능하다.

인텔리 J 컴파일 방법 : 메뉴 build => Recompile
```



<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49573?tab=curriculum&q=107977)