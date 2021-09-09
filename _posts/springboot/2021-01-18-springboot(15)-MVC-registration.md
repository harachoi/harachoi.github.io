---
title: Spring boot기반 Web Application 개발[15] - 회원 등록
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

 [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(14)-MVC-home/)에서 간단한 홈 화면을 구축했다. 이어서 **Form**을 이용해 회원등록을 진행해보자. 

# 회원 웹 기능 - 등록

## 회원 등록 폼 컨트롤러

**controller** 폴더에 `MemberController.java` 파일을 추가하고, 아래 코드를 작성하자.

![image](https://user-images.githubusercontent.com/49560745/104889462-9e02d000-59b1-11eb-8dc5-0d98b8c7319f.png)

```
파일명 : MemberController.java
위치 : \main\hello.hellospring\controller\MemberController.java
```

```java
package hello.hellospring.controller;

import hello.hellospring.domain.Member;
import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @GetMapping("members/new")
    public String createForm(){
        return "members/createMemberForm";
    }
}
```

- `members/new` 라는 `url` 요청이 들어오면 `members/createMemberForm` 화면을 렌더링 한다.
- `url` 요청은 `@GetMapping` 인 **Get** 방식으로 이루어진다는 점을 기억하자.

## 회원 등록 폼 HTML 

**templtes** 폴더 아래 **members** 폴더를 생성하자. 그리고 `createMemberForm.html` 파일을 생성하고 아래 코드를 작성하자. 

![image](https://user-images.githubusercontent.com/49560745/104889835-16699100-59b2-11eb-88d2-f609949427b9.png)

```
파일명 : createMemberForm.html
위치 : \main\resources\templates\members\createMemberForm.html
```

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <form action="/members/new" method="post">
        <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" name="name" placeholder="이름을
입력하세요">
        </div>
        <button type="submit">등록</button>
    </form>
</div> <!-- /container -->
</body>
</html>
```

## 웹 등록 화면에서 데이터를 전달 받을 폼 객체

**controller** 폴더에 `MemberForm.java` 파일을 추가하고, 아래 코드를 작성하자. **Form** 데이터를 담기위한 객체를 생성하는 것이다.

![image](https://user-images.githubusercontent.com/49560745/104890032-592b6900-59b2-11eb-98a2-c23ee0b62e46.png)

```
파일명 : MemberForm.java
위치 : \main\hello.hellospring\controller\MemberForm.java
```

```java
package hello.hellospring.controller;

public class MemberForm {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

## 회원 컨트롤러에서 회원을 실제 등록하는 기능

**controller** 폴더에 `MemberController.java` 파일을 추가하고, `@PostMapping`애노테이션 코드를 추가해주자.

![image](https://user-images.githubusercontent.com/49560745/104889462-9e02d000-59b1-11eb-8dc5-0d98b8c7319f.png)

```
파일명 : MemberController.java
위치 : \main\hello.hellospring\controller\MemberController.java
```

```java
package hello.hellospring.controller;

import hello.hellospring.domain.Member;
import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @GetMapping("members/new")
    public String createForm(){
        return "members/createMemberForm";
    }

    @PostMapping("members/new")
    public String create(MemberForm form){
        Member member=new Member();
        member.setName(form.getName());
        memberService.join(member);

        return "redirect:/";
    }
}
```

- `@GetMapping`과 `@PostMapping`의 **mapping**이 겹치지만, **mapping** 애노테이션의 타입이 다르기 때문에 가능하다.
- 주로 `@GetMapping`은 요청할 떄, `@PostMapping`은 데이터를 전달할 때 사용한다.
- `createMemberForm.html` 파일에서 `<form action="/members/new" method="post">` form 데이터 전송 형식이 `post` 이기 때문에 `@PostMapping`이 요청을 가로챈다.

<br/>

# 동작 과정

**1) 홈 화면**

`localhost:8000` 에 접속하면 홈 화면이 렌더링된다.

![image](https://user-images.githubusercontent.com/49560745/104908789-4e7cce00-59ca-11eb-99b7-8c40e7ee7c98.png)

**2) 등록 폼 화면**

`회원가입` 버튼을 클릭하면, 등록 폼 화면으로 이동한다. `@GetMapping`애노테이션이 요청을 가로챈다. 

![image](https://user-images.githubusercontent.com/49560745/104909351-11650b80-59cb-11eb-9b81-203328b28921.png)

**3) form.getName()**

등록 버튼을 누른 후 console 창에 name을 `print()` 해보자. from에 입력한 이름과 일치하는 것을 확인할 수 있다. 이때, 브라우저는 홈 화면으로 `redirect` 된다.

![image](https://user-images.githubusercontent.com/49560745/104909422-2d68ad00-59cb-11eb-969e-8b3462019607.png)

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

