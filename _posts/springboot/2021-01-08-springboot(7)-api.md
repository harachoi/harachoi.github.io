---
title: Spring boot기반 Web Application 개발[7] - API
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at:
---



Spring boot Web 개발에서 이야하기는 **API**방식을 알아보자.

이전에 살펴봤던 [정적컨텐츠](https://gwang920.github.io/spring%20boot/springboot(5)-static/), [템플릿 엔진](https://gwang920.github.io/spring%20boot/springboot(6)-mvc/)과 같이 사용자에게 보여지는 데이터를 반환하는 방식 중 하나를 **API** 방식이라고한다.

```
정적컨텐츠 - 정적으로 HTML 파일을 그대로 Client(브라우저)에 리턴한다.
템플릿엔진(MVC) - View를 랜더링 된 HTML을 Client(브라우저)에 전달해준다.
API 방식 - 주로 객체를 JSON형태로 리턴하고자할 떄 사용하는 방식이다.
```

# API 방식

**API** 방식은 `@ResponseBody`를 사용한다는 점을 기억하고, 실습을 진행해보자.



## 1) String 형식반환

이전에 진행했던 `HelloController.java`파일에 이어서 아래 코드를 작성하자.

```java
@GetMapping("hello-string")
@ResponseBody
public String helloString(@RequestParam("name") String name){
    return "hello" + name;
}
```

- `url` 매핑 "hello-string"
- `@ResponseBody` 태그 : 메소의 반환 값을 그대로 브라우저에게 반환한다.

```
파일명 : HelloController.java
위치 : src\main\java\hello.hellospring\controller\HelloController.java
```

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String Hello(Model model){
        model.addAttribute("data","hello!!");
        return "hello";
    }

    @GetMapping("hello-mvc")
    public String helloMVC(@RequestParam("name") String name,Model model){
        model.addAttribute("name",name);
        return "hello-template";
    }
	
    // 추가된 코드
    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name){
        return "hello" + name;
    }
}
```



 `http://localhost:8000/hello-string?name=spring!!!!!!!!` 접속해보자.

![image](https://user-images.githubusercontent.com/49560745/103976878-4d25f700-51bb-11eb-82bf-1388c6f802be.png)

정상적으로 로드된다. 그렇다면 `템플릿엔진` 혹은 `정적 컨텐츠` 방식과는 어떤차이가 있을까?

화면에서 우클릭을하고 **페이지 소스 보기**를 눌러보자.

![스프링부트 API 방식 페이지 로드](https://user-images.githubusercontent.com/49560745/103976988-8d857500-51bb-11eb-90f4-ce1191caf9c4.png)

`HTML` 코드가 보이지 않는다. 이게 바로 Spring boot에서 이야기하는 **API** 방식이다.

## 2) JSON 형식 반환

한 가지 예제를 더 들어보자.

```java
@GetMapping("hello-api")
@ResponseBody
public Hello helloApi(@RequestParam("name") String name){
      Hello hello =new Hello();
      hello.setName(name);
      return hello;
    }

static class Hello{
    private String name;

    public String getName() {
            return name;
    }

	public void setName(String name) {
           this.name = name;
       }
    }
```

**Hello**라는 Class를 만들고(`static` : Class내 Class 선언 가능), 객체를 생성한 후 반환해보자.

```
파일명 : HelloController.java
위치 : src\main\java\hello.hellospring\controller\HelloController.java
```

````java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String Hello(Model model){
        model.addAttribute("data","hello!!");
        return "hello";
    }

    @GetMapping("hello-mvc")
    public String helloMVC(@RequestParam("name") String name,Model model){
        model.addAttribute("name",name);
        return "hello-template";
    }

    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name){
        return "hello " + name;
    }

    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name){
        Hello hello =new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello{
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}

````

서버를 재실행하고, 위 `http://localhost:8000/hello-api?name=spring!!!!!!!!`에 접속해보자.

![image](https://user-images.githubusercontent.com/49560745/103977220-1b616000-51bc-11eb-9a65-e8b71a22051d.png)

`{key: value}`쌍의 `JSON` 데이터 반환되는 것을 확인할 수 있다. 이처럼 **API** 방식은 객체 자체를 return 해줄 수 있고, 이를 `JSON` 형식으로 변환해준다.



## @ResponseBody 원리

`@ResponseBody`는 HTTP의 BODY에 문자 내용을 직접 반환한다. `@ResponseBody`가 동작하는 원리는 다음과 같다.

![@ResponseBody 원리](https://user-images.githubusercontent.com/49560745/103977627-00432000-51bd-11eb-9e10-4a706e76904c.png)

```
1) 웹 브라우저 요청
ex) localhost:8000/hello-string?name=spring
	localhost:8000/hello-api?name=spring
	
2) 내장 톰캣 서버, 컨텐츠 찾기 
  스프링 컨테이너 접근
  => helloController에 "hello-string" / "hello-api" 매핑 존재
  => @ResponseBody 태그 발견
  
3) return 형태에 따라 
	HttpMessageConverter 선택
	
	객체 : JsonConverter 동작
	문자열 : StringConverter 동작
	
* [참고] - 클라이언트의 HTTP Accept 해더와 서버의 컨트롤러 반환 타입 정보 
둘을 조합해서 HttpMessageConverter 가 선택된다.

4) 객체 혹은 문자열을 브라우저에 그대로 반환

```

이외에도 사용자 설정에 따라 `XML`, Google의 `Gson`형식으로 return 할 수 있다. 현업에서는 이 부분은 거의 건드리지 않고, `default`로 사용한다고 한다. `default` 설정은 아래와 같다. 참고로 `Jackson2`는 범용적으로 사용되는 검증된 `JSON`라이브러리이다.

```
기본(default) 문자처리 : StringHttpMessageConverter 동작
기본(default) 객체처리 : MappingJackson2HttpMessageConverter 동작
```

<br/>

**[참고] Accept 요청 HTTP 헤더**

Accept 요청 HTTP 헤더는 MIME 타입으로 표현되는,클라이언트가 이해 가능한 타입이 무엇인지 알려준다. MIME(Multiple Internet Mail Extensions)은 바이트의 문서, 파일, 형식의 표준이다.

- 문법

```
Accept: <MIME_type>/<MIME_subtype>
Accept: <MIME_type>/*
Accept: */*

// Multiple types, weighted with the quality value syntax:
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8
```

- 예제

```
Accept: text/html
Accept: image/*
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8
```



<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

- [Accept - MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Accept)

  

