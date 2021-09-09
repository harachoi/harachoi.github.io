---
title: Spring boot기반 Web Application 개발[10] - 회원 서비스 개발
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---





**Service**는 비즈니스 로직을 수행한다. 더 서비스 로직에 가까워 보이는 것들이 **Service**에 구현 된다. [회원 레포지토리 개발 포스팅](https://gwang920.github.io/spring%20boot/springboot(8)-requirements/) 에서 `repository`에 구현했던 `MemoryMemberRepository.java` 의 메소드와의 차이를 비교해보는 것도 좋을 것 같다.

**비즈니스 로직이란?**

```
비즈니스 로직은 컴퓨터 프로그램에서 실세계의 규칙에 따라 
데이터를 생성·표시·저장·변경하는 부분을 일컫는다. 
이 용어는 특히 데이터베이스, 표시장치 등 
프로그램의 다른 부분과 대조되는 개념으로 쓰인다.
```

[[출처] 비즈니스 로직 - 위키백과](https://ko.wikipedia.org/wiki/%EB%B9%84%EC%A6%88%EB%8B%88%EC%8A%A4_%EB%A1%9C%EC%A7%81)

위키백과에 따르면 **비즈니스 로직**은 실세계의 규칙에 따라 데이터를 생성·표시·저장·변경 하는 것이라고 한다. 서비스 이용자(유저)가 원하는 행위를 응용 프로그램이(ex - Web 애플리케이션) 바르게 수행할 수 있도록 하기 위해 반드시 거쳐야하는 과정이다. 

쉽게 말해, **버그**없이 애플리케이션이 동작하도록 검증하는 로직이라고 할 수 있지 않을까(?)

 이해를 돕기 위해 추가적으로 설명하자면, **Controller**는 브라우저(사용자)의 **Request**를 어떻게 처리할지 고민한다면, **Service**는 브라우저(사용자)의 요청에 대해 어떤 처리를 할 것인가를 고민한다. **중복 회원 방지**나 **비밀번호 일치 확인** 등을 **Service**의 예로 들 수 있다. 

이번 포스팅에서는 **Service** 로직을 구현해볼 것이다.

# MemberService

### Join

 회원과 관련 된 **Service** 로직을 구현해보자. 아래와 같이 `service` 패키지를 추가하고, `MemberService.java` 클래스 파일을 생성하자. 그리고 `join - 회원가입` 비즈니스 로직을 구현해보자.

![image](https://user-images.githubusercontent.com/49560745/104154498-68496e80-5428-11eb-98fc-00a8b3f1dba8.png)

```
파일명 : MemberService.java
위치 : \src\main\java\hello.hellospring\service\MemberService.java
```

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;


public class MemberService {

    private final MemoryMemberRepository memberRepository
						=new MemoryMemberRepository();

    /**
     * 중복 회원 관리
     */
    public Long join(Member member){
        memberRepository.findName(member.getName())
                .ifPresent(m->{
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
        memberRepository.save(member);
        return member.getId();
    }
}

```

- `Name`이 중복된다면 `IllegalStateException` 처리
- `IllegalStateException` 을 통과하면 회원 정보 저장

**[참고]** 

아래 두 코드는 완벽하게 **동일**하다. `Optional`은 여러 메소드를 실행할 수 있는데 그 중 하나가
 `ifPresent()`이다. 이 메소드를 활용해 이미 존재하는 정보를 판단할 수 있다.

```java
- code 1
  memberRepository.findName(member.getName())
           .ifPresent(m->{
               throw new IllegalStateException("이미 존재하는 회원입니다.");
           });
 -------------------------------------------------------------------------        
- code 2
  Optional<Member> result=memberRepository.findName(member.getName());
        result.ifPresent(m->{
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });
 -------------------------------------------------------------------------
```

 서비스 로직 수행 메소드에서 세부 기능은 따로 분리하는게 좋다. 

```java
memberRepository.findName(member.getName())
	.ifPresent(m->{
		throw new IllegalStateException("이미 존재하는 회원입니다.");
	});
```

 코드를 드래그 한 뒤, **전구** 표시를 클릭하면 코드를 `refactoring`할 수 있다. 여기서, `Extract Method`를 클릭하고 아래와 같은 화면이 나오면, **Name**을 설정해주자. 

![image](https://user-images.githubusercontent.com/49560745/104155020-a004e600-5429-11eb-8e49-cc893317992f.png)

자동으로 설정한 **validateDuplicateMember**라는 이름의 메소드가 생성된 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/49560745/104155399-77c9b700-542a-11eb-9b08-273822770492.png)



### findMembers

 전체 회원을 조회하는 메서드를 추가하자.

```java
    /**
     * 회원 전체 조회
     */
    public List<Member> findMembers(){
        return memberRepository.findAll();
    }
```

- List로 반환

### findOne

 아이디로 회원 정보를 조회하자.

```java
    /**
     * 회원 아디로 조회
     */
    public Optional<Member> findOne(Long memeberId){
        return memberRepository.findID(memeberId);
    }

```

- 일치하는 `id` 의 Member 객체 반환



## 전체코드

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;


public class MemberService {

    private final MemoryMemberRepository memberRepository=new MemoryMemberRepository();

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

 기본적인 **service**의 비즈니스로직을 구현했다. 다음 포스팅에서 마찬가지로 `junit`을 활용해 `Test`를 진행해보자.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

- [리다양 - SpringBoot Controller, Service, DAO 이해 - Service(1)](https://onlyformylittlefox.tistory.com/13)