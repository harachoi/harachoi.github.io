---
title: Spring boot기반 Web Application 개발[16] - 회원 목록 조회
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

 [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(15)-MVC-registration/)에서 간단한 회원 등록 기능을 구현했다. 이어서 등록된 회원을 조회할 수 있는 페이지를 만들어보자. 

# 회원 웹 기능 - 조회

## 회원 조회 메서드

**controller** 폴더에 `MemberController.java` 파일에 아래 코드를 추가하자.

```java
@GetMapping("/members")
public String list(Model model){
	List<Member> members = memberService.findMembers();
	model.addAttribute("members",members);
	return "members/memberList";
}
```

- `addAttribute`로 렌더링할 객체를 담아준다.
- `memeberList.html`에서 회원 목록을 렌더링 할 수 있도록 `url`을 지정해 return 해준다.

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
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import java.util.List;

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
        System.out.print(form.getName());
        memberService.join(member);
        return "redirect:/";
    }

    @GetMapping("/members")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members",members);
        return "members/memberList";
    }
}

```



## 회원 목록 조회 HTML 

`memberList.html` 파일을 생성하고 아래 코드를 작성하자. 

![image](https://user-images.githubusercontent.com/49560745/104992076-9439a500-5a63-11eb-9742-5a96811975e8.png)

```
파일명 : memberList.html
위치 : \main\resources\templates\members\memberList.html
```

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <table>
            <thead>
            <tr>
                <th>#</th>
                <th>이름</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="member : ${members}">
                <td th:text="${member.id}"></td>
                <td th:text="${member.name}"></td>
            </tr>
            </tbody>
        </table>
    </div>
</div> <!-- /container -->
</body>
</html>

```

- 넘어온 `members` 객체의 `id`, `name` 값을 순차적으로 렌더링한다. 



<br/>

# 동작 과정

이제 실제로 회원을 등록해보고, 회원 목록을 조회해보자.

**1) 회원 등록**

**spring1**, **spring2**를 이름으로 회원등록

![image](https://user-images.githubusercontent.com/49560745/104992257-feeae080-5a63-11eb-82e4-0c94640b78fb.png)

![image](https://user-images.githubusercontent.com/49560745/104992235-f4304b80-5a63-11eb-92b4-4cb66492d39a.png)

**2) 회원 목록 조회**

`회원 목록` 버튼을 클릭하면, 회원 목록이 렌더링된다.

![image](https://user-images.githubusercontent.com/49560745/104992306-188c2800-5a64-11eb-9c4a-0590d18b0714.png)

**3) 페이지 소스보기**

`membersList.html` 에서 `each` 문으로 `members` 객체를 `iteration` 하면서 `member.id` 와 `member.name`이 테이블의 요소로 생성된 후 렌더링되는 것이다.

![image](https://user-images.githubusercontent.com/49560745/104992386-496c5d00-5a64-11eb-9aa3-74e773d2ec98.png)

지금까지  `MVC`를 활용해 홈 화면, 회원 등록, 회원 목록 조회 기능을 구현했다. `HashMap` 자료구조를 적용해 저장소를 구현했는데, 다음 포스팅에서는 이제 진짜 **데이터베이스**를 적용해 저장소를 구현해볼 것이다.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

