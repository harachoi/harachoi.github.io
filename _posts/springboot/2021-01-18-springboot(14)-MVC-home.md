---
title: Spring boot기반 Web Application 개발[14] - 홈 화면 추가
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

 이전 포스팅에서 **컨트롤러**, **서비스**, **리포지토리**의 의존관계 설정을 마무리했다. 이번 포스팅에서는 본격적으로 **MVC**를 활용해 애플리케이션을 제어해보자. 

# 회원 웹 기능 - 홈 화면 추가

## 홈 컨트롤러 추가

**controller** 폴더에 `HomeController.java` 파일을 추가하고, 아래 코드를 작성하자.

![image](https://user-images.githubusercontent.com/49560745/104886559-50846400-59ad-11eb-820b-13d2ff0ce647.png)

````
파일명 : HomeController.java
위치 : \main\hello.hellospring\controller\HomeController.java
```

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

## 회원 관리용 홈

**templtes** 폴더 아래 `home.html` 파일을 생성하고 아래 코드를 작성하자. 정적 컨텐츠가 아닌 `thymeleaf` 템플릿 엔진을 사용하기 위해 **templates** 아래에 파일을 생성한다.

![directory](https://user-images.githubusercontent.com/49560745/104886499-36e31c80-59ad-11eb-9f80-ed3dcec097e8.png)

```
파일명 : home.html
위치 : \main\resources\templates\home.html
```

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div> <!-- /container -->
</body>
</html>

```

이제, 애플리케이션을 구동해보자.

![springboothome](https://user-images.githubusercontent.com/49560745/104886765-9fca9480-59ad-11eb-93c2-887304d365f6.png)

정상적으로 `home.html`이 로드된다. 

그런데 여기서 의문점이 하나 생긴다. `/` 경로로 접근하면 최초로 `index.html`이 로드된다고 알고 있다. 하지만, `index.html` 이 아닌 `templates/home.html` 이 로드되고 있다. 스프링 부트의 동작방식을 보며 이해해보자.

![스프링부트 정적컨텐츠 동작원리](https://user-images.githubusercontent.com/49560745/103514630-5c9dfb00-4eb0-11eb-8f92-98e15afdc9f0.png)

`url` 요청이 들어오면, 최초로 스프링 컨테이너에서 `controller`에서 일치하는 **mapping**을 찾는다. 알맞은 **mapping**이 존재하지 않는다면 이때서야 `static`폴더에 접근해 해당하는 파일을 return한다. 따라서, 우선순위에 앞서는 `home.html`이 로드된 것이라 할 수 있다. 자세한 내용은 [스프링부트 정적컨텐츠](https://gwang920.github.io/spring%20boot/springboot(5)-static/)를 참고하자.

<br/>

# 오류

- finished with non-zero exit value 1

bootRun에 관련된 에러이므로

```
netstat -ao |find /i "listening"
```

현재 스프링 부트 톰캣 포트인 8000(Default : 8080)포트를 사용하고 있는 **PID**를 알아내야한다.

![netstat -ao |find /i "listening"](https://user-images.githubusercontent.com/49560745/104885804-fa62f100-59ab-11eb-9cd7-1ca8dd770faa.png)

아래 명령어를 입력해 **PID 7020** 작업을 종료해주자.

```
Taskkill /F /IM 7020
```

![TaskKill](https://user-images.githubusercontent.com/49560745/104885953-3e55f600-59ac-11eb-8823-762003b5ab9e.png)

이제 다시 스프링 애플리케이션을 구동하면 정상동작한다. 다음 포스팅에서는 **Form**을 이용해 회원을 등록해보자.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)
- [non-zero 에러](https://dreamday.tistory.com/entry/string-boot-finished-with-nonzero-exit-value-1-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95)

