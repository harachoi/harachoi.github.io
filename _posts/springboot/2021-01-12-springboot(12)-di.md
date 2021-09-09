---
title: Spring boot기반 Web Application 개발[12] - 의존성 주입(DI)
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

  의존성 주입(DI)을 통해 **컨트롤러**, **서비스**, **레파지토리**의 의존관계를 설정해보자. 의존성 주입으로 컨트롤러가 서비스, 레파지토리를 사용할 수 있다.

# 의존성 주입(DI) - 컴포넌트 스캔

 이번 포스팅에서는 지금까지 구현해놓았던 **MemberService**, **MemberRepository**를 **MemberController**에서 사용할 수 있도록 의존성을 주입해보려고 한다.

### 의존성 주입(Dependency Injection)이란 ?

```
소프트웨어 엔지니어링에서 의존성 주입은 
하나의 객체가 다른 객체의 의존성을 제공하는 테크닉이다. 
"의존성"은 예를 들어 서비스로 사용할 수 있는 객체이다. 
클라이언트가 어떤 서비스를 사용할 것인지 지정하는 대신, 
클라이언트에게 무슨 서비스를 사용할 것인지를 말해주는 것이다.
```

[[출처] 의존성 주입 - 위키백과](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85)

의존성 주입은 쉽게 말해, 필요한 객체를 직접 생성하는 것이 아니라 외부에서 직접 설정하는 것이다. 즉, 외부로부터 필요한 객체를 받아서 사용한다. [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(11)-serviceTest/)에서 **생성자**를 통해 간단하게 의존성 주입을 훑어봤다. 본격적으로 의존성 주입에 대해 알아보자.

의존성 주입의 장점과 방법을 간단하게 소개하면 다음과 같다.

### 의존성 주입 장점

-  코드 재활용성이 높아진다
- 객체간의 결합도를 낮추고 코드를 유연하게 작성할 수 있다.

### 의존성 주입 방법

- 컴포넌트 스캔과 자동 의존관계 설정
- 자바 코드로 직접 스프링 빈 등록하기

 이번 포스팅에서는 **컴포넌트 스캔**을 활용해 의존성을 주입해볼 것이다.

## MemberController

**회원 컨트롤러**가 **회원서비스**와 **회원 리포지토리**를 사용할 수 있게 의존관계를 준비하자. `controller` 패키지에 `MemberController.java` 클래스를 생성하고, 아래 코드를 작성하자.

```
파일명 : MemberController.java
위치 :  main\java\hello.hellospring\controller\MemberController.java
```

```java
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

- `@Controller` 어노테이션은 해당 클래스가 controller 클래스라고 명시하는 것이다.
- `@Autowired` 어노테이션은 자동으로 의존관계를 만들어, memberservice와 연결시켜준다.
- 생성자에 `@Autowired` 어노테이션이 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.

이제, main Application인 `HelloSpringApplication.java` 를 실행해보자.

![스프링부트 실행 오류 : non-zero exit value 1](https://user-images.githubusercontent.com/49560745/104301874-7c19d100-550b-11eb-8931-e3eaad038910.png)

오류가 발생한다. **memberSerivce**가 스프링 빈으로 등록되어있지 않기때문이다.

![스프링 컨테이너](https://user-images.githubusercontent.com/49560745/104302586-50e3b180-550c-11eb-8c15-fcff961932ca.png)

`MemberController` 에서 `MemberService` 에 의존성을 주입하려했지만, 스프링 컨테이너에서 `MemberService` 를 찾을 수 없다. `MemberService` 에도 마찬가지로 어노테이션 설정을 통해 스프링 컨테이너에 등록해야한다.

## MemberService

**MemberService** 에도 `@Service` 어노테이션, 생성자에는 `@Autowired`  어노테이션을 추가하자.

```
파일명 : MemberService.java
위치 : main\java\hello.hellospring\service\MemberService.java
```

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class MemberService {

    private final MemoryMemberRepository memberRepository;

    @Autowired
    public MemberService(MemoryMemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }


    /**
     * 중복 회원 관리
     */
    public Long join(Member member){
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findName(member.getName())
                .ifPresent(m->{
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    /**
     * 회원 전체 조회
     */
    public List<Member> findMembers(){
        return memberRepository.findAll();
    }

    /**
     * 회원 아디로 조회
     */
    public Optional<Member> findOne(Long memeberId){
        return memberRepository.findID(memeberId);
    }

}

```

## MemoryMemberRepository

**MemoryMemberRepository** 에도 `@Repository`어노테이션, 생성자에는 `@Autowired`  어노테이션을 추가하자.

```
파일명 : MemoryMemberRepository.java
위치 : main\java\hello.hellospring\repository\MemoryMemberRepository.java
```

```java
package hello.hellospring.respository;

import hello.hellospring.domain.Member;
import org.springframework.stereotype.Repository;

import java.util.*;

@Repository
public class MemoryMemberRepository implements MemberRepository{
    private static Map<Long,Member> store= new HashMap<>();
    private static long sequence =0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(),member);
        return member;
    }

    @Override
    public Optional<Member> findID(long id) {
        return Optional.ofNullable(store.get(id));
    }

    /**
     Map을 순회하기 위해, iterator() 대신에 stream()을 사용하고,
     filter()로 조건에 맞는지 판단한다.
     findAny()는 비어 있는 스트림에서는 비어있는 Optional 객체를 반환한다.

     * iterator()
     컬렉션에 저장되어 있는 요소들을 읽어오는 방법 중의 하나
     **/
    @Override
    public Optional<Member> findName(String Name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(Name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore(){
        store.clear();
    }
}

```

이제, 다시 실행해보자.

![스프링부트 실행완료](https://user-images.githubusercontent.com/49560745/104302252-ed598400-550b-11eb-8ad8-b4249dba8736.png)

정상적으로 구동된다.

![스프링 컨테이너 controller service repository](https://user-images.githubusercontent.com/49560745/104302989-d49d9e00-550c-11eb-8d52-ce8c713d228b.png)

스프링 컨테이너 내에 **memberService**와 **memberRepository** 스프링 빈이 등록되었기 때문에 위 그림처럼 **service**, **repository**에 접근할 수 있다.

## 컴포넌트 스캔 원리

그렇다면 컴포넌트 스캔은 어떻게 이루어질까? 

- `@Component` 애노테이션이 있으면, 스프링 빈으로 자동 등록된다.

- `@Controller` 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔때문이다.

- `@Component` 를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다.
  ```
  @Controller
  @Service
  @Repository
  ```

  

`@Controller`를 `ctrl` + `click` 해보자. `@Controller`는 이미 `@Component`에 포함되어있다. 이때문에 스프링 자동설정에 따라 스프링 빈에 등록되는 것이다. `@Service` , `@Repository`도 마찬가지다.

![@Component @Controller](https://user-images.githubusercontent.com/49560745/104303246-365e0800-550d-11eb-81f7-7584b68f9bcc.png)



### 스프링 빈 - 싱글톤

스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 **싱글톤**으로 등록한다. 

**싱글톤**

```
전역 변수를 사용하지 않고 객체를 "하나"만 생성 하도록 하며, 
생성된 객체를 "어디에서든지 참조"할 수 있도록 하는 패턴
```

Singleton Scope

```
Defining a bean with singleton scope means 
the container "creates a single instance of that bean", 
and all requests for that bean name will 
return "the same object", which is cached. 
Any modifications to the object will be reflected 
in all references to the bean. 
This scope is the default value if no other scope is specified.
```

[스프링 빈 - 싱클톤 스코프](https://www.baeldung.com/spring-bean-scopes)

유일하게 하나만을 등록해 공유한다.**(Cached 됨)** 따라서, 같은 스프링 빈이면 모두 같은 인스턴스다. 예를들어, **OrderService**에서 만약 **MemberRepository** 의 객체가 필요하다면, 새롭게 생성하는 것이 아니라 이미 등록되어있는 **MemberRepository**에 접근하는 것이다. 즉, **OrderService**와 **MemberService**는 같은 **레퍼런스를** 공유한다.

![스프링 부트 - 싱글톤](https://user-images.githubusercontent.com/49560745/104309540-ac666d00-5515-11eb-90c2-bfc33f96f3c6.png)

그렇기에 앞서 말했던 의존성 주입의 장점인 **코드의 재사용성이 높아지고, 자원 활용의 효율이 높아지는** 것이다.  참고로, 설정으로 싱글톤이 아니게 설정할 수 있지만, 현업에서 특별한 경우를 제외하면 대부분 싱글톤을 사용한다고 한다.

다음 포스팅에서 자바 코드로 직접 스프링 빈을 등록해보자.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)
- https://www.baeldung.com/spring-bean-scopes

