---
title: Spring boot기반 Web Application 개발[5] - 정적 컨텐츠
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at:
---



Spring boot를 활용해 **정적 컨텐츠**에 대해 간단하게 알아보자.

# 정적 컨텐츠란?

```
변화가 없는 컨텐츠를 말한다. 보통 HTML,CSS,Img,javascript 파일 등 미리
서버에 저장해두고, 요청을 받으면 그저 응답만 해주면 되는 것들이다.
초창기 Web에서는 서버의 성능이 떨어져, Web 어플리케이션을
정적 컨텐츠로만 구성했다.

하지만, 최근에는 'FaceBook 친구 추천'이나 '유튜브 동영상 추천'기능
처럼 동적으로 컨텐츠를 생성하는 경우가 많아,
대부분 정적 + 동적 컨텐츠로 Web 어플리케이션을 구축하고 있다.
```

Spring boot에서는 정적 컨텐츠 기능을 제공한다.

자세한 내용은 [Spring Boot Features](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)에서 "static content"를 검색해보자.

<br/>

# 정적 컨텐츠 생성

정적 컨텐츠를 생성하는 방법은 정말 간단하다.

**/static** 폴더 내에 파일을 생성하고, 해당 위치로 접근하면 정적 컨텐츠가 로드된다.

```
파일명 : hello-static.html
위치 : resources/static/hello-static.html
```

```html
<!DOCTYPE HTML>
<html>
<head>
 <title>static content</title>
 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

```
실행 : localhosts:8000/hello-static.html
```

![image](https://user-images.githubusercontent.com/49560745/103514425-ee593880-4eaf-11eb-92d9-1513505d909f.png)

# 정적 컨텐츠 동작 원리

**정적 컨텐츠**가 사용자 요청으로부터 로드되는 과정은 다음과 같다.

![스프링부트 정적컨텐츠 동작원리](https://user-images.githubusercontent.com/49560745/103514630-5c9dfb00-4eb0-11eb-8f92-98e15afdc9f0.png)

```
1) 웹 브라우저 요청
ex) localhost:8000/hello-static.html

2) 내장 톰캣 서버, 컨텐츠 찾기 
  1 - 스프링 컨테이너 접근
  => hello-static 관련 컨트롤러가 존재하지 않음
  
  2 - static 폴더 접근
  => hello-static.html 존재
  
3) "hello-static.html" 브라우저로 전송
```

<br/>

# 정적 컨텐츠 더 알아보기

## 정적 리소스 mapping URL 패턴

![image](https://user-images.githubusercontent.com/49560745/103530335-8e23c000-4eca-11eb-9322-05f6a5d55778.png)



![image](https://user-images.githubusercontent.com/49560745/103526482-56b21500-4ec4-11eb-9daa-0480519d9475.png)



정적 리소스는 기본적으로 `/**(루트)` 부터 매핑이 된다. 따라서, 위 예시처럼 `localhost:8000/hello-static.html`을 요청하면 정적 리소스의 location(위치)에서 정적 컨텐츠를 찾아 응답한다.



### mapping 패턴 변경하기

* mapping 이란?

```
- 해당 하는 값이 다른 값을 가르키도록 하는 것.
- 주소가 간결해지는 장점이 있다.

기존 경로 : https://localhost:8000/main/controller/test
매핑 경로 : https://localhost:8000/main/cTest
```

<br/>

Spring Boot에서는 `/resources/appication.properties` 파일에 정적 리소스 매핑 패턴을 설정할 수 있다.

매핑 패턴은 `srping.mvc.static-path-pattern=/` 이후에 경로를 설정하여 변경가능하다. (default는 `srping.mvc.static-path-pattern=/**`)이다.  

<br/>

mappig 패턴을 `/static/**`으로 바꿔보자.

```
spring.mvc.static-path-pattern=/static/**
```



![image](https://user-images.githubusercontent.com/49560745/103527310-7dbd1680-4ec5-11eb-837e-fea53e950c65.png)



<br/>

그리고 요청을 보내보자. `hello-static.html` 파일은 여전히 패턴 변경 전과 동일한 `/static` 에 위치하고 있지만, `localhost:8000/hello-static.html` 을 요청하면 **Error page** 가 로드된다.

![Whitelabel Error Page](https://user-images.githubusercontent.com/49560745/103527665-0b990180-4ec6-11eb-8037-c0a3cdbdb48d.png)

<br/>

**appication.properties**에 설정한 매핑 패턴으로 접근해야 비로소 `hello-static.html`이 로드됨을 확인할 수 있다.



![image](https://user-images.githubusercontent.com/49560745/103526597-7f3a0f00-4ec4-11eb-8d75-1f149582f135.png)





* [참고] 이 프로퍼티는 **WebMvcProperties**에 구현되어 있다.

<br/>

## 정적 리소스 Location

기본적으로 Spring boot는 정적 컨텐츠를 `/staic` or`/public` or `/resources` or `/META-INF/resources` 로 부터 불러온다. 즉,  4가지 경로는 정적 리소스의 위치를 뜻한다.

```
classpath:/META-INF/resources/
classpath:/resources/
"classpath:/static/"
"classpath:/public/"
```

<br/>

### 정적 리소스 Location 변경 하기

정적 리소스의 위치를 `/new-static/` 으로 변경해보자.

우선, `resources` 하위 경로에 `new-static` 폴더를 생성하자. 

![image](https://user-images.githubusercontent.com/49560745/103537333-933b3c00-4ed7-11eb-997b-fd4d9d4cf1c8.png)

<br/>

그리고 새로운 `hello-static.html` 파일 을 생성하자.

![image](https://user-images.githubusercontent.com/49560745/103529313-caeeb780-4ec8-11eb-8184-bd0cfcd4d7e1.png)

<br/>

`/resources/appication.properties` 에 `new-static` 폴더를 정적 리소스의 Location으로 설정하자. `spring.resources.static-locations=classpath:` 이후에 경로를 지정할 수 있다.

```
spring.resources.static-locations=classpath:/new-sattic/
```



![image](https://user-images.githubusercontent.com/49560745/103529832-b52dc200-4ec9-11eb-9387-f3986c360963.png)



<br/>

그리고 `localhost:8000/hello-static.html`을 요청하면 정상적으로 html이 로드됨을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/49560745/103529806-a810d300-4ec9-11eb-9dd7-1d8af4a5bc1f.png)





하지만, 이 방법은 한 가지 **문제**가 있다. `spring.resources.static-locations=classpath:` 으로 location을 설정하는 방법은 스프링 부트에서 기본으로 제공하는 locations를 **override**하기 때문에 설정 후에는 기본 location을 사용할 수 없다. 아래 예시를 보자.

위에서 설정한 `new-static` 폴더를 제거하자.

![image](https://user-images.githubusercontent.com/49560745/103530738-510bfd80-4ecb-11eb-96ed-c9a68842e59a.png)

<br/>

그리고 `localhost:8000/hello-static.html`에 요청을 보내면 아래 처럼 **Error Page**가 로드된다.

![image](https://user-images.githubusercontent.com/49560745/103530772-5e28ec80-4ecb-11eb-8ca3-434f7f9abec1.png)

<br/>

 `spring.resources.static-locations=classpath:/new-static` 이  `spring.resources.static-locations=classpath:/` 를 **Override** 했기 때문이다.

![image](https://user-images.githubusercontent.com/49560745/103530898-9defd400-4ecb-11eb-8992-70a0876aac7e.png)

<br/>

### 정적 리소스 설정

**WebMvcConfigurer**를 구현하는 클래스에서 **addResourceHandlers**를 **override**하여 정적 리소스 핸들러를 새롭게 설정할 수 있다. 위 **override** 문제를 해결하는 방법이다. 즉, 스프링 부트에서 **default**로 제공되는 정적 리소스 핸들러는 그대로 사용하면서 **사용자 커스텀 핸들러가 추가**되는 것이다.

<br/>

우선, `/java/hello.hellospring/config` 위치에 `WebConfig` class 와 `/resources/` 밑에 `test`폴더와 그 아래 `static.html` 을 아래와 같이 생성하자.



![image](https://user-images.githubusercontent.com/49560745/103537033-f6789e80-4ed6-11eb-8bdd-b9e557c2e00c.png)

```
파일명 : WebConfig.java
```

```java
package hello.hellospring.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/test/**")
                .addResourceLocations("classpath:/test/")
                .setCachePeriod(20) // 캐시가 얼마나 지속될지 결정한다.
        ;
    }
}
```

**@Override** 태그에 빨간줄이 뜨면 import가 되지 않아 발생하는 것이다.[IntelliJ 자동 import](https://hjjungdev.tistory.com/102)를 참고하자.



```
파일명 : static.html
```

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
WebMvcConfigurer로 WebConfig 설정하기
</body>
</html
```

<br/>

설정한 경로를 요청하면 page가 load 된다.

![스프링부트 정적컨텐츠 로드](https://user-images.githubusercontent.com/49560745/103535769-a8fb3200-4ed4-11eb-9a8e-6f02b9337d9d.png)

<br/>

기본 경로를 요청해도 page가 load 됨을 확인할 수 있다.

![스프링부트 정적 컨텐츠 로드](https://user-images.githubusercontent.com/49560745/103536884-a13c8d00-4ed6-11eb-90f2-c9c2726e2511.png)

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49576?tab=curriculum)
- [도움코딩 - 정적콘텐츠]((https://armontad-1202.tistory.com/entry/%EC%9B%B9%EC%9D%98-%EC%A0%95%EC%A0%81-%EB%8F%99%EC%A0%81-%EC%BD%98%ED%85%90%EC%B8%A0-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%A0%95%EC%A0%81-%EB%8F%99%EC%A0%81-%EC%BD%98%ED%85%90%EC%B8%A0))
- [Knowledge Repository 정정컨텐츠 커스텀](https://atoz-develop.tistory.com/entry/spring-boot-web-mvc-static-resources)

