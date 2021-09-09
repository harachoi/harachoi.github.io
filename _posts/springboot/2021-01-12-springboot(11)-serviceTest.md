---
title: Spring boot기반 Web Application 개발[11] - 회원 서비스 테스트(Junit)
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

 [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(10)-service/)에서 구현했던 **service**를 테스트해보자. 마찬가지로 자바 테스트 프레임워크 `Junuit`을 활용한다. 

# 테스트 클래스 자동 생성

 이번에는 단축키를 이용하여 **테스트** 클래스를 만들어보자. 우선, **테스트**가 필요한 **service** 클래인 `MemberService.java`로 가보자. 그리고 단축키 `ctrl` + `shift` + `t`를 클릭하고, `Create new Test` 버튼을 클릭하자.

![스프링부트 태스트 클래스 생성](https://user-images.githubusercontent.com/49560745/104272273-1fed8780-54e0-11eb-83b4-e1713166ac9f.png) 

보는 것처럼 **class name**을 자동으로 설정해준다. 아래 `Member` 태그아래 **테스트**하고자 하는 메소드 **check box**를 클릭하자.

![image](https://user-images.githubusercontent.com/49560745/104272400-5a572480-54e0-11eb-90af-7be3e4800a6c.png)

`MemberSerivceTest.java`가 자동 생성된 것을 확인할 수 있다. 

![junit test](https://user-images.githubusercontent.com/49560745/104273935-16b1ea00-54e3-11eb-9ac5-de592a7ebf1c.png)

# Join Test

이제 테스트코드를 작성해보자. **join** 기능부터 시작한다. 아래 코드를 `MemberSerivceTest.java`에 작성해주자. 기본적인 **join** 기능은 [회원 리포지터리 테스트](https://gwang920.github.io/spring%20boot/springboot(9)-repositoryTest/)에서 정상동작 함을 확인했을 것이다. 이번에 **비즈니스 로직** 테스트에 중점을 두고, `중복_회원_예외` 메서드를 작성해보자.

### try - catch

```
파일명 : MemberSerivceTest.java
위치 : \test\java\hello.hellospring\service\MemberSerivceTest.java
```

````java
   MemberService memberService=new MemberService();

    @Test
    void 회원가입() {
        // given
        Member member=new Member();
        member.setName("spring");

        //when
        Long saveId=memberService.join(member);

        //then
        Member findmember=memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findmember.getName());

    }

    @Test
    public void 중복_회원_예외(){
        //given
        Member member1=new Member();
        member1.setName("spring");

        Member member2=new Member();
        member2.setName("spring");
        //when
        memberService.join(member1);
        try {
            memberService.join(member2);
            fail();
        }catch (IllegalStateException e){
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
        }
        //then
    }
````

- 테스트코드에서는 메서드 이름을 **한글**로 작성해도 문제없다.
- `[given - when - then]` 형식으로 코드를 작성해보자.
- 두 객체를 생성하고, 같은 이름을 넣어보자
  - 이때, 실패가 나와야 옳은 결과이다.
  -  `try - catch` 문을 활용해 실패했을 때, 나오는 메시지가 일치하는지 확인해보자.

![junit 테스트 성공](https://user-images.githubusercontent.com/49560745/104274393-1b2ad280-54e4-11eb-822b-8173b6cffbe0.png)

정상적인 결과가 나온다.

### assertThrows

 `try - catch` 문을 더 간결하게 표현할 수 있다.

```java
try {
	memberService.join(member2);
	fail();
}catch (IllegalStateException e){
	assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
}
```

위 `try - catch` 문을 아래와 같이 변경해보자. 두 코드는 동일하게 동작한다.

````java
assertThrows(IllegalStateException.class,()-> memberService.join(member2));
````

- `assertThrows(오류명.class(), 실행 메소드)` 형식으로 지정할 수 있다.

```
파일명 : MemberSerivceTest.java
위치 : \test\java\hello.hellospring\service\MemberSerivceTest.java
```

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.assertj.core.api.Assertions.failBecauseExceptionWasNotThrown;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {

    MemberService memberService=new MemberService();

    @Test
    void 회원가입() {
        // given
        Member member=new Member();
        member.setName("spring");

        //when
        Long saveId=memberService.join(member);

        //then
        Member findmember=memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findmember.getName());
    }

    @Test
    public void 중복_회원_예외(){
        //given
        Member member1=new Member();
        member1.setName("spring");

        Member member2=new Member();
        member2.setName("spring");
        //when
        memberService.join(member1);
        assertThrows(IllegalStateException.class,()-> memberService.join(member2));
		assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

    }

    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```

![junit 테스트 성공](https://user-images.githubusercontent.com/49560745/104275019-672a4700-54e5-11eb-919f-041b57be1801.png)

정상동작한다.

테스트 코드가 정상동작하는지 확인하기 위해서는 그 반대 기능도 테스트해봐야한다. `IllegalStateException` 에러 구문을 가장 익숙한 `NullPointerException`으로 바꿔보자.

````java
assertThrows(NullPointerException.class,()-> memberService.join(member2));
````

![junit 테스트 실패](https://user-images.githubusercontent.com/49560745/104275314-ea4b9d00-54e5-11eb-9778-b80642aa70fc.png)

**fail**이 나온다. `MemberService.java`에서 던진 에러 `IllegalStateException`와 다르기 때문이다.따라서, 테스트 코드는 정상적으로 동작한다고 할 수 있다.

### getMessage()

메시지도 테스트해볼 수 있다. 코드를 살짝 변경해보자.

```java
assertThrows(IllegalStateException.class, () -> memberService.join(member2));
```

이 코드를 아래와 같이 바꿔보자.

````java
IllegalStateException e = assertThrows(IllegalStateException.class, 
() -> memberService.join(member2));
assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
````

- `throw` 에러에서 추출 된 메시지가 일치하는지 테스트
- `alt` + `enter`를 입력하고, `split into declarations and assignment`를 클릭하면 자동으로 지역변수(위 코드에서 **IllegalStateException e**)가 생성된다.

![junit 테스트 성공](https://user-images.githubusercontent.com/49560745/104276859-10267100-54e9-11eb-802f-2bfcd6807b17.png)

`message` 테스트도 정상적으로 실행된다.

## 통합 테스트

테스트는 항상 테스트 코드가 실행된 후, 다시 예전 상태로 돌려놔야한다. 그래야만 다른 테스트가 정상적으로 동작하는지 판단할 수 있다. 예를들어,  `TestA` 테스트 메소드 실행 시 DB에 `A`라는 객체를 삽입했다면, `TestB`메소드를 실행되기 전에 `A`라는 객체를 다시 삭제해줘야한다. **테스트**전의 상태로 되돌리는 것이다.

위에서 작성한 `MemberSerivceTest.java` 에서 통합 테스트(Class 테스트)를 실행하면 오류가 발생할 것이다. 위와 같은 문제 때문이다. 이와 관련하여 실습해보자. 아래코드를 `MemberSerivceTest.java`에 추가하고, 테스트를 실행해보자.

  ```java
MemoryMemberRepository memoryMemberRepository= new MemoryMemberRepository();

@AfterEach
public void afterEach(){
	memoryMemberRepository.clearStore();
}
  ```

- `memoryMemberRepository` 인스턴스는 가상의 `db`인 `repository`이다. 
- `@AfterEach` 어노테이션을 추가하면, 매 테스트 코드가 실행된 후에 `afterEach()` 메소드가 실행된다.
  - `db`를 비워주는 작업과 동일하다고 생각하면 좋다.

![junit 전체 테스트 성공](https://user-images.githubusercontent.com/49560745/104277933-22a1aa00-54eb-11eb-8307-18fa7ae99de2.png)

정상동작한다.

## 생성자 - 의존성 주입(DI)

하지만, 위 코드도 한 가지 문제점이 있다. `new` 키워드를 통해 **Test class** 내에 `repository`를 생성하고 있다. 다시말해, `MemberService.java`에 있는 `memberRepository` 인스턴스와 `MemberServiceTest.java`에 있는 `memberRepository` 인스턴스가 서로 다르다. 다른 저장소로 작업을 하고 있는 것이다. 동일한 저장소에 접근하도록 변경해보자.

우선, `MemberService.java` 에서 `new` 생성자를 삭제해주고 아래 코드를 추가하자.

```java
public MemberService(MemoryMemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
```

- 생성자를 통해 인스턴스의 저장소를 할당해준다.

```
파일명 : MemberService.java
위치 : main\java\hello.hellospring\service\MemberService.java
```

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;


public class MemberService {

    private final MemoryMemberRepository memberRepository;

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

다음으로 `MemberServiceTest.java` 에 `new` 생성자를 삭제하고, 아래 코드를 삽입하자.

```java
@BeforeEach
public void beforeEach(){
	memoryMemberRepository=new MemoryMemberRepository();
	memberService=new MemberService(memoryMemberRepository);
}
```

- `@BeforeEach` 어노테이션은 `@AfterEach`와 반대이다. 테스트 메소드를 실행하기전에 무조건 실행된다.
- `MemberService.java` 클래스와 `MemberServiceTest.java` 클래스는 이제 동일한 `repository`를 갖게 된다.
- 테스트가 독립 실행되는 것을 보장할 수 있다.

```
파일명 : MemberServiceTest.java
위치 : \test\java\hello.hellospring\service\MemberSerivceTest.java
```

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {

    MemberService memberService;
    MemoryMemberRepository memoryMemberRepository;

    @BeforeEach
    public void beforeEach(){
        memoryMemberRepository=new MemoryMemberRepository();
        memberService=new MemberService(memoryMemberRepository);
    }

    @AfterEach
    public void afterEach(){
        memoryMemberRepository.clearStore();
    }

    @Test
    void 회원가입() {
        // given
        Member member=new Member();
        member.setName("spring");

        //when
        Long saveId=memberService.join(member);

        //then
        Member findmember=memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findmember.getName());
    }

    @Test
    public void 중복_회원_예외(){
        //given
        Member member1=new Member();
        member1.setName("spring");

        Member member2=new Member();
        member2.setName("spring");
        //when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");


    }


    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```

지금 이 상황은 `MemberService.java` 의 생성자가 외부 동작(Test 코드의 BeforeEach)에 의해 프로퍼티가 결정된다고 얘기할 수 있다. 이것이 바로 **의존성 주입(DI)**이다. 말그대로 외부에서 의존성을 주입해주는 것이다.

**의존성 주입(DI)** 과 관련 된 자세한 내용은 다음 포스팅에서 다뤄봐야겠다.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)

