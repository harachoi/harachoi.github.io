---
title: Spring boot기반 Web Application 개발[9] - 회원 리포지터리 테스트(Junit)
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---



  자바는 테스트를 할 때, 주로 테스트 프레임워크인 `Junit`을 활용한다. 그런데 한 가지 의문이 든다. 자바의 **main 메서드**를 이용하거나 **웹 애플리케이션의 컨트롤러**를 통해 충분히 테스트가 가능한데, 왜 **테스트 프레임워크**를 이용해서 테스트를 진행할까? 

# Junit 장점

```
- 테스트 준비시간을 단축할 수 있다.
- 한 번에 여러 테스트를 진행할 수 있다.
```

그 이유는 바로 위 방법들은 준비하고, 실행하는데 **시간이 오래걸리기** 때문이다. 그뿐만 아니라 **반복실행이 어렵다**는 단점이 있다. 그렇기 때문에, 여러 테스트를 한번에 실행하기에는 곤란하다. 따라서,  현업에서 규모있는 프로젝트를 구축하거나, 많은 사람이 참여하는 프로젝트에서는 `Junit`을 활용해 애플리케이션을 테스트한다. 

이제 본격적으로 테스트 코드를 작성해보자.

## Save() Test

 그럼 이제, 앞서 구현했던 회원 `repository`를 테스트해보자. 테스트는 `Junit`으로 진행한다. 우선, `main` 디렉토리가 아닌 `test` 디렉토리에 `respository` 패키지를 생성하자. 그리고 `MemoryMemberRepositoryTest.java` 클래스를 생성하자.

![image](https://user-images.githubusercontent.com/49560745/104150231-91fc9880-541c-11eb-8e5d-4db77a4fcc2b.png)

```
파일명 : MemoryMemberRepositoryTest.java
위치 : \src\test\java\hello.hellospring.repository\MemoryMemberRepositoryTest.java
```

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

import java.util.Optional;

public class MemoryMemberRepositoryTest {

    MemoryMemberRepository memoryMemberRepository= new MemoryMemberRepository();

    @Test
    public void save(){
        Member member=new Member();
        member.setName("spring");

        memoryMemberRepository.save(member);

        Member result = memoryMemberRepository.findID(member.getId()).get();
        Assertions.assertThat(result).isEqualTo(result);
    }

}

```

- save() 기능을 테스트
- `MemoryMemberRepository`객체로 저장소 생성
- `Member` 객체를 생성해 저장할 회원 정보를 **setting**
- 저장했던 객체의 `id`와 실제로 `findID()` 메서드를 통해 찾아온 `id`가 일치하는지 확인 
- `Assertions(org.junit.jupiter.api)`을 활용해 두 값이 일치 확인 

테스트는 `15 line` 초록색 세모 아이콘을 클릭하자. 메서드 별로 실행 가능

![image](https://user-images.githubusercontent.com/49560745/104150683-02f08000-541e-11eb-8f01-b221ced6d448.png)

![image](https://user-images.githubusercontent.com/49560745/104150194-73969d00-541c-11eb-87a3-491aee0e93d2.png)

정상적으로 테스트가 완료되었음을 확인할 수 있다.

## FindByName() Test

이름으로 회원정보가 정상적으로 이루어지는 확인해보자. 아래 코드를 `MemoryMemberRepositoryTest.java`파일에 추가하자.

````java
    @Test
    public void findByName(){
        Member member1=new Member();
        member1.setName("spring1");
        memoryMemberRepository.save(member1);

        Member member2=new Member();
        member2.setName("spring2");
        memoryMemberRepository.save(member2);

        Member result=memoryMemberRepository.findName(member1.getName()).get();
        Assertions.assertThat(result).isEqualTo(member1);
    }
````

```
파일명 : MemoryMemberRepositoryTest.java
위치 : \src\test\java\hello.hellospring.repository\MemoryMemberRepositoryTest.java
```

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

import java.util.Optional;

public class MemoryMemberRepositoryTest {

    MemoryMemberRepository memoryMemberRepository= new MemoryMemberRepository();

    @Test
    public void save(){
        Member member=new Member();
        member.setName("spring");

        memoryMemberRepository.save(member);

        Member result = memoryMemberRepository.findID(member.getId()).get();
        Assertions.assertThat(result).isEqualTo(result);
    }

    @Test
    public void findByName(){
        Member member1=new Member();
        member1.setName("spring1");
        memoryMemberRepository.save(member1);

        Member member2=new Member();
        member2.setName("spring2");
        memoryMemberRepository.save(member2);

        Member result=memoryMemberRepository.findName(member1.getName()).get();
        Assertions.assertThat(result).isEqualTo(member1);
    }

}

```

- `member1`, `member2` 객체를 생성하고 각각 `spring1` , `spring2`로 이름 설정
- `repository`에 저장하고, 마찬가지로 `Assertions(org.junit.jupiter.api)`로 테스트 진행

![image](https://user-images.githubusercontent.com/49560745/104151137-8c548200-541f-11eb-8779-675b1c54174f.png)

정상적으로 동작하는 것을 확인할 수 있다.

## SelectAll() Test

마지막으로 모든 회원 정보가 `select`되는지 확인해보자. 아래 코드를 `MemoryMemberRepositoryTest.java`파일에 추가하자.

```java
    @Test
    public void findAll(){
        Member member1=new Member();
        member1.setName("spring1");
        memoryMemberRepository.save(member1);

        Member member2=new Member();
        member2.setName("spring2");
        memoryMemberRepository.save(member2);

        List<Member> list=memoryMemberRepository.findAll();
        Assertions.assertThat(list.size()).isEqualTo(2);
    }
```

```
파일명 : MemoryMemberRepositoryTest.java
위치 : \src\test\java\hello.hellospring.repository\MemoryMemberRepositoryTest.java
```

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import hello.hellospring.respository.MemoryMemberRepository;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.Optional;

public class MemoryMemberRepositoryTest {

    MemoryMemberRepository memoryMemberRepository= new MemoryMemberRepository();

    @Test
    public void save(){
        Member member=new Member();
        member.setName("spring");

        memoryMemberRepository.save(member);

        Member result = memoryMemberRepository.findID(member.getId()).get();
        Assertions.assertThat(result).isEqualTo(result);
    }

    @Test
    public void findByName(){
        Member member1=new Member();
        member1.setName("spring1");
        memoryMemberRepository.save(member1);

        Member member2=new Member();
        member2.setName("spring2");
        memoryMemberRepository.save(member2);

        Member result=memoryMemberRepository.findName(member1.getName()).get();
        Assertions.assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll(){
        Member member1=new Member();
        member1.setName("spring1");
        memoryMemberRepository.save(member1);

        Member member2=new Member();
        member2.setName("spring2");
        memoryMemberRepository.save(member2);

        List<Member> list=memoryMemberRepository.findAll();
        Assertions.assertThat(list.size()).isEqualTo(2);
    }

}

```

- `member1`, `member2` 두 개의 객체를 생성
- list로 `selectAll()` 결과 값을 받아오고, `size()` 와 비교

![image](https://user-images.githubusercontent.com/49560745/104151293-fec56200-541f-11eb-99e1-4b24511fd65a.png)

모든 회원정보조회도 완벽하게 구현되었음을 알 수 있다. 

## 모든 테스트 케이스 실행

이번에는, `테스트 프레임워크`의 장점을 활용해보자. 모든 테스트 코드를 한 번에 실행할 수 있다. 실행은 `MemoryMemberRepositoryTest` 클래스의 **테스트 실행 버튼**을 누르자.

![image](https://user-images.githubusercontent.com/49560745/104151928-8bbceb00-5421-11eb-8e49-6b573bac115b.png)

![image](https://user-images.githubusercontent.com/49560745/104151954-9d9e8e00-5421-11eb-99f3-debb513888a3.png)

개별적인 테스트 코드는 모두 정상적으로 동작했는데 **오류**가 발생한다. 이유는 테스트 코드 메서드는 순차적으로 실행되지 않기 때문이다. 각 메서드가 중복되는 객체를 `repository`에 넣어주고 있고, 여기서 오류가 발생하는 것이다. 이를 해결하려면 어떻게 해야할까?

방법은 간단하다. 매 테스트 메서드를 실행하고, `repository`를 비워주기만하면 된다. [이전 포스팅](https://gwang920.github.io/spring%20boot/springboot(8)-requirements/)에서 `clearStore()`메서드를 구현했었다. 이를 활용해보자.

![image](https://user-images.githubusercontent.com/49560745/104151569-be1a1880-5420-11eb-8100-93cea36fff1f.png)

아래 코드를 `MemoryMemberRepositoryTest.java`파일에 추가하자. `@AfterEach`는 매 테스트 케이스 실행 후에 동작하도록하는 `annotation`이다.

```java
    @AfterEach
    public void afterEach(){
        memoryMemberRepository.clearStore();
    }
```

그리고 다시 테스트를 실행해보자.

![image](https://user-images.githubusercontent.com/49560745/104152183-287f8880-5422-11eb-838f-52a3fe06bcb8.png)

모든 테스트 케이스가 정상적으로 실행되는 것을 확인할 수 있다.

<br/>

# vs TDD

위에서 진행했던 테스트는 TDD와는 다르다. 그냥 코드를 작성하고, 테스트를 진행한 것에 불과하다. 요즘 개발 트렌드로서 `TDD`가 주목받고 있다. `TDD` 는 테스트 코드를 먼저 장석하고, 이에 부합하도록 기능을 개발하는 것이다. 기회가 된다면 `TDD`에 관해서도 포스팅해 볼 생각이다.

```
테스트 주도 개발(Test-driven development TDD)은 
매우 짧은 개발 사이클을 반복하는 소프트웨어 개발 프로세스 중 하나이다. 
개발자는 먼저 요구사항을 검증하는 자동화된 테스트 케이스를 작성한다. 
그런 후에, 그 테스트 케이스를 통과하기 위한 최소한의 코드를 생성한다. 
마지막으로 작성한 코드를 표준에 맞도록 리팩토링한다. 

이 기법을 개발했거나 '재발견' 한 것으로 인정되는 
Kent Beck은 2003년에 TDD가 
단순한 설계를 장려하고 자신감을 불어넣어준다고 말하였다.
```

- [[출처] TDD - 위키백과](https://ko.wikipedia.org/wiki/%ED%85%8C%EC%8A%A4%ED%8A%B8_%EC%A3%BC%EB%8F%84_%EA%B0%9C%EB%B0%9C)

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49577?tab=curriculum)


