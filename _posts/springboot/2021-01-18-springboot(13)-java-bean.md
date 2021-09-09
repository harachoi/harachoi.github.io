---
title: Spring boot기반 Web Application 개발[13] - 의존성 주입(DI)
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

  [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(12)-di/)에서 **컴포넌트 스캔** 방식으로 스프링 빈을 등록했다. 이번 포스팅에서는 자바 코드로 스프링 빈을 등록해보자. 

```
일반적으로 실무에서 정형화된 컨트롤러, 서비스, 리포지토리 
같은 코드는 컴포넌트 스캔을 사용한다고 한다. 
그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 
설정을 통해 스프링 빈으로 등록한다.
```

[참고] 김영한님의 스프링-입문-스프링부트

[이번 어플케이션 개발 요구사항](https://gwang920.github.io/spring%20boot/springboot(8)-requirements/)에서 '저장소는 아직 정해지지 않았다.' 라고 가정했었다. 그렇기 때문에 클래스 변경에 유연한 **자바 코드로 스프링 빈**을 등록해보자.

# 의존성 주입(DI) - 자바 빈 

 자바 코드로 스프링 빈을 등록하는 방법은 간단하다. `@Configuration`, `@Bean` 애노테이션을 사용하면 설정할 수 있다. 우선, 스프링 빈을 직접 등록하기 위해 이전에 작업했던 애노테이션을 제거해주자. `MemberService.java` 와 `MemoryMemberRepository.java` 파일에서 `@Service`, `@Respository`, `@Autowired` 애노테이션을 삭제하자.

### Repository

```
파일명 : MemoryMemberRepository.java
위치 : main\hello.hellospring\repository\MemoryMemberRepository.java
```

```java
package hello.hellospring.respository;

import hello.hellospring.domain.Member;
import org.springframework.stereotype.Repository;

import java.util.*;

//@Repository 제거
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

### Service

`MemberService.java` 파일에서는 `@Service` `@Autowired` 애노테이션과 코드를 변경해주자. 기존 코드는 주석처리하고, 변경이 필요한 코드를 삽입했다. 그대로 수정해주자.

```
파일명 : MemberService.java
위치 : main\hello.hellospring\service\MemberService.java
```

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

//@Service 제거
public class MemberService {

    //private final MemoryMemberRepository memberRepository; 변경
	private final MemberRepository memberRepository;
    
	//@Autowired 제거
/*    
	public MemberService(MemoryMemberRepository memberRepository) {

        this.memberRepository = memberRepository;
    }
*/
    
    public MemberService(MemberRepository memberRepository) {

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

그리고 스프링 애플리케이션을 실행하면 오류가 발생한다. `MemberController.java` 에서 등록되어있지 않은 `Memberservice` 객체를 스프링 컨테이너에서 찾으려고 하기 때문이다. 자바 코드로 스프링 빈을 등록하여 해결해보자.

## 자바 코드로 스프링 빈 등록

애노테이션을 제거했다면, 이제 `hello.hellospring` 폴더 아래에 `SpringConfig.java` 클래스 파일을 생성하고 아래 코드를 작성해보자.

```
파일명 : SpringConfig.java
위치 : main\hello.hellospring\SpringConfig.java
```

```java
package hello.hellospring;

import hello.hellospring.respository.MemberRepository;
import hello.hellospring.respository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}

```

`@Configuration` 애노테이션은 스프링 빈을 수동으로 등록하는 클래스를 명시하는 것이다. `@Bean`은 스프링 빈으로 등록하고자하는 메소드에 지정한다. 위 코드를 작성하면 **리파지토리** 와 **서비스**를 스프링 빈 등록이 완료되었다고 볼 수 있다. 애플리케이션을 구동해보면 이전 코드와 동일하게 정상 작동하는 것을 확인할 수 있다.

이제, **메모리 리포지토리**를 **다른 리포지토리**로 변경가능한 상태가 만들어졌다. 

![스프링 컨테이너 controller service repository](https://user-images.githubusercontent.com/49560745/104302989-d49d9e00-550c-11eb-8d52-ce8c713d228b.png)

몇 가지 **참고할 사항**이다.

- XML로 설정하는 방식도 있지만 최근에는 잘 사용하지 않는다.
- DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다. 

**필드 주입**

필드 주입을 하면 코드의 양이 줄어드는 장점이있다. 아래 코드처럼 객체 선언에 앞서 `@Autowired` 애노테이션을 설정하면 된다.

```java
package hello.hellospring.service;
import hello.hellospring.respository.MemberRepository;
import hello.hellospring.respository.MemoryMemberRepository;

@Service
public class MemberService {
    /*
    	필드 주입방식은 final 키워드를 사용할 수 없다.
    	그렇기에 코드가 변경될 위험이 있다.
    */
	 @Autowired private  MemberRepository memberRepository;
}

```



**setter 주입**

호출해야되지 않을 메서드가 호출되면 곤란하다. **setter 주입** 방식의 문제점이다. 조립시점에 한 번만 생성하고 변경하지 못하도록 해야하지만, **setter**를 통해 변경이 가능하다.(public으로 노출된다.)

```java
package hello.hellospring.service;
import hello.hellospring.respository.MemberRepository;
import hello.hellospring.respository.MemoryMemberRepository;

@Service
public class MemberService {

    private MemberRepository memberRepository;
    
	@Autowired
    public setMemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}

```



**생성자 주입**

생성자 주입은 조립 시점에 최초로 한번 설정되면 동적으로 변경되지 않는다. 가장 권장되는 방식이다. 불변 필드(final)은 생성자 주입을 통해서만 가능하다.

```java
package hello.hellospring.service;
import hello.hellospring.respository.MemberRepository;
import hello.hellospring.respository.MemoryMemberRepository;

@Service
public class MemberService {

    private final MemberRepository memberRepository;
	
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}

```



- 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다. 그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다. 
- @Autowired 를 통한 DI는 helloConroller , memberService 등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemberRepository;
import hello.hellospring.respository.MemoryMemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;


public class MemberService {

    private final MemberRepository memberRepository;
	
    // class인 MemberService 객체를 스프링 컨테이너에 등록하지 않았기에 오류가 발생한다.
	@Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

}

```



<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)
- https://www.baeldung.com/spring-bean-scopes

