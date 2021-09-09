---
title: Spring boot기반 Web Application 개발[8] - 요구사항 & 회원 도메인, 리포지터리
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---



 지금까지 Spring boot 기반 Web 개발에 대한 기초를 알아봤다. 이제 본격적으로 간단한 회원시스템을 만들어보자. 개발은 항상 **요구사항**을 토대로 진행된다. 앞으로 개발할 시스템의 요구사항은 다음과 같다.

# 요구사항

```
데이터: 회원ID, 이름
기능: 회원 등록, 조회
아직 데이터 저장소가 선정되지 않음(가상의 시나리오)
```

 여기서 **가상의 시나리오**는 실제 현업에서 개발할 때, `NoSQL` , `RDB`등 저장소를 선정하기전에 개발을 진행해야 할때가 종종 있어 설정해둔 것이라고 한다. 간단하게 회원 ID와 이름만을 가지고 백엔드 시스템을 설계해보자.

**일반적인 웹 애플리케이션의 계층 구조**

![스프링부트 웹 애플리케이션 계층구조](https://user-images.githubusercontent.com/49560745/103983127-1d312080-51c8-11eb-8e3c-94fa415c942e.png)

- 컨트롤러: 웹 MVC의 컨트롤러 역할 
- 서비스: 핵심 비즈니스 로직 구현 
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리 
- 도메인: 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

 **컨트롤러**는 웹 MVC에서 말하는 **컨트롤러**와 같다. **모델**과 **뷰**사이에서 모든 요청을 관리해준다. **서비스**에는 핵심 비즈니스 로직이 구현된다. **서비스**의 예로는 회원가입 시 중복체크나 로그인 비밀번호 일치 확인 등이 있다. 아직 `Database`가 결정되지 않았기 때문에 **리포지토리**를 사용해 간단하게 데이터를 주고 받도록 요구사항이 설계되었다.

**클래스 의존관계**

![스프링 부트 클래스 의존관계](https://user-images.githubusercontent.com/49560745/103983531-bceeae80-51c8-11eb-8cef-68317644cd9a.png)

- 범용성을 고려해, 인터페이스로 구현 클래스를 변경할 수 있도록 설계
  - 참고로 인터페이스는 껍데기만을 구현해 놓은 것으로 개발 시 요구사항에 알맞게 내용물을 구현하라고 강제해놓는 것이다.
- 데이터 저장소는 `RDB`, `NoSQL` 등등 다양한 저장소를 고민중인 상황으로 가정 
- 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용

<br/>

# 구현

본격적으로 코드를 작성해보자. 기존에 진행했던 Project에 이어서 작업하면 된다.

![image](https://user-images.githubusercontent.com/49560745/103985535-756a2180-51cc-11eb-902d-47f861138677.png)

이번 포스팅에서 최종 구현 된 디렉토리 구조이다.

# domain

우선, `hello.hellospring` 하위 경로로 `domain` 패키지를 생성하자.그리고 `Member class(Member.java)`를 생성하자. 

```
파일명 : Member.java
위치 : src\main\java\hello.hellospring\domain\Member.java
```

```java
package hello.hellospring.domain;

public class Member {

    private long id;
    private String name;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

앞선 요구사항에서 `id`와 `name` 만을 데이터로 사용한다고 정의했기에 이에 따라 클래스 변수 `id`, `name` 과 `getter`, `setter` 메소드를 생성하자. 이 `Member class`를 통해 데이터를 객체형태로 주고받을 것이다.

# repository

다음으로 `repository` 패키지를 생성하자. 하위 경로에는 `MemberRepository class(MemberRepository.java)` 인터페이스와 `MemoryMemberRepository class(MemoryMemberRepository.java)`를 생성하자.

```
파일명 : MemberRepository.java
위치 : src\main\java\hello.hellospring\repository\MemberRepository.java
```

 ```java
package hello.hellospring.respository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findID(long id);
    Optional<Member> findName(String Name);
    List<Member> findAll();
}

 ```

이 파일은 인터페이스로, 수정이나 확장성을 고려해 내용물 없이 **껍데기**만을 구현해놓은 것이다. 이를 `implements`해 내용물을 구현할 것이다.

- save 메소드 : 회원정보를 저장한다.
- findID, findName 메소드 : 각각 id, name을 통해 회원정보를 조회한다.
- findAll 메소드 : 저장소에 저장 된 모든 회원정보를 조회한다.



```
파일명 : MemoryMemberRepository.java
위치 : src\main\java\hello.hellospring\repository\MemoryMemberRepository.java
```

```java
package hello.hellospring.respository;

import hello.hellospring.domain.Member;

import java.util.*;

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

인터페이스 `MemberRepository`를 `implements`해 내용물을 구현했다.

- Map : 임시 저장소로 사용한다. `<id , Member>`의 `<key: value>` 형식으로 데이터를 저장한다.
- sequence : 회원정보 저장 시 자동으로 카운트 +1
- Optional : 앞서 인터페이스 구현시 `Optional`을 사용했다. `Optional`은 `Java8`에서 도입된 기능으로 `null`이나 `null`이외의 값을 담을 수 있다. 

이제, 코드가 정상적으로 동작하는지 테스트를 해봐야한다. 테스트는 다음 포스팅에서 `Junit`을 사용해 구현해 볼 예정이다.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

- [Accept - MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Accept)

  

