---
title: Spring boot기반 Web Application 개발[6] - MVC와 템플릿엔진
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at:
---



Spring boot를 활용해 **MVC**와 **템플릿 엔진**에 대한 간단한 정의를 알아보고, 어떠한 방식으로 동작되는지 실습해보자.

# MVC란?

```
모델-뷰-컨트롤러(Model–View–Controller, MVC)는 
소프트웨어 공학에서 사용되는 소프트웨어 디자인 패턴이다. 

이 패턴을 성공적으로 사용하면, 
사용자 인터페이스로부터 '비즈니스 로직'을 분리하여 
애플리케이션의 '시각적 요소'나 그 이면에서 실행되는 '비즈니스 로직'을 
서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다. 

MVC에서 
'모델'은 애플리케이션의 정보(데이터)를 나타내며, 
'뷰'는 텍스트, 체크박스 항목 등과 같은 사용자 인터페이스 요소를 나타내고, 
'컨트롤러'는 데이터와 비즈니스 로직 사이의 상호동작을 관리한다.
```

[출처 - 위키백과]



**MVC패턴**은 Spring 혹은 JSP 등을 활용해 Web 어플리케이션을 구축해봤다면, 익숙할만한 개념이다. **MVC패턴**은 소프트웨어 디자인 패턴 중에 하나로써, 서버 뒷단의 로직을 처리하는 **비즈니스 로직**과 브라우저에서 화면을 만들어 주는 **시각적 요소**를 따로 분리해 효율적으로 Web 어플리케이션을 개발할 수 있도록 한다. 



<br/>

# 템플릿엔진이란?

```
html(Markup)과 데이터를 결한한 결과물을 만들어 주는 도구
```

 웹 서비스를 개발하면서 JSP를 사용해본적이 있을 것이다. JSP는 HTML 코드 내에 `<% %>`태그로 JAVA코드를 삽입 해 웹 서비스를 개발할 수 있는 서버사이드 스크립트언어이다. 하지만, 이 방식은 html 코드와 JAVA 코드가 혼재되어 서비스를 관리하거나 규모 있는 웹 서비스를 구축하기에는 어려움이있다. 이를 보완해주는 도구가 **템플릿 엔진**이다. Spring boot에서는 템플릿엔진으로 `Thyme leaf`를 사용한다는 점을 기억하자.

<br/>

이제, **MVC**와 **템플릿 엔진**을 활용해 [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(5)-static/)에 이어서 간단한 실습을 진행해보자.

 

# Controller

기존에 작성했던 `HellocController.java`파일에 아래코드를 구현하자.

```java
@GetMapping("hello-mvc")
    public String helloMVC(@RequestParam("name") String name,Model model){
        model.addAttribute("name",name);
        return "hello-template";
    }
```

- `url`매핑은 `@GetMapping`어노테이션을 활용해 "hello-mvc"로 설정했다. 

- `@RequestParam`어노테이션을 활용해 `url`의 name 값을 가져온다. 

- `model.addAttrubute`로 'name' 값을 Html 파일에 넘겨준다.

- hello-template라는 View 파일을 return 해준다.

<br/>

```
파일명 : HellocController.java
위치 : src\main\java\hello\hellospring\controller\HelloController.java
```

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String Hello(Model model){
        model.addAttribute("data","hello!!");
        return "hello";
    }
	// 추가된 코드
    @GetMapping("hello-mvc")
    public String helloMVC(@RequestParam("name") String name,Model model){
        model.addAttribute("name",name);
        return "hello-template";
    }
}

```

<br/>

# View

Controller에서 return 해주는 View 파일을 만들어보자. `resources > templates`에 `hello-template.html`파일을 생성하고, 아래 코드를 작성하자.

```
파일명 : hello-template.html
위치 : src\main\resources\templates\hello-template.html
```

````html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
````

- <html xmlns:th="http://www.thymeleaf.org">  : thymeleaf 템플릿 엔진을 사용하기 위함이다.
- `${name}`은 `HellocController.java1`의 helloMVC 메소드에서 구현 한 model.addAttribute("name",name); 으로 설정된다.

<br/>

# 실행

우선, https://http://localhost:8000/hello-mvc에 접속해보자. 

![Whitelabel Error Page](https://user-images.githubusercontent.com/49560745/103727961-f20bcd00-501f-11eb-8f65-469a51d196e2.png)

**Error Page**가 로드될 것이다. 매핑된 "hello-mvc"에 접근했지만, `@RequestParam`에서 설정한 name값이  `url`에존재하지 않기 때문이다.

<br/>

다시, https://http://localhost:8000/hello-mvc?name=spring 으로 접속해보자. 여기서 name은 파라미터 인 "spring"으로 설정되었다.

![image](https://user-images.githubusercontent.com/49560745/103728125-778f7d00-5020-11eb-84c4-b29b795dd7cc.png)

이제, 정상적으로 html 파일이 로드되는 것을 확인할 수 있다.

<br/>

# MVC 동작 원리

**Controller**에 의해 **MVC**가 동작하는 과정은 다음과 같다.

![스프링부트 MVC 동작원리](https://user-images.githubusercontent.com/49560745/103728221-b6bdce00-5020-11eb-8c72-f6011c6e84ab.png)

```
1) 웹 브라우저 요청
ex) localhost:8000/hello-mvc

2) 내장 톰캣 서버, 컨텐츠 찾기 
  스프링 컨테이너 접근
  => helloController에 "hello-mvc" 매핑 존재
  
3) model을 viewResolver에 전달 (파라미터 => name: spring)

4) hello-template.html 반환 (Theymeleaf 템플릿 엔진 처리) 
```



<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [위키백과 MVC패턴](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC)

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

  

