---
title: Spring boot기반 Web Application 개발[2] - 라이브러리 살펴보기
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

Spring boot에서 사용되는 라이브러리를 알아보자.

![image](https://user-images.githubusercontent.com/49560745/103196974-7e373980-4928-11eb-9a07-23c20745a224.png)

Intellij에서 우측에 Gradle을 클릭하면 **의존관계**에 있는 모든 라이브러리를 확인할 수 있다. Gradle은 **의존관계**가 있는 라이브러리를 함께 다운로드 한다. 

## 스프링 부트 라이브러리 

````
spring-boot-starter-web
	- spring-boot-starter-tomcat: 톰캣 (웹서버)
	- spring-webmvc: 스프링 웹 MVC
	
spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
	- spring-boot
		- spring-core
	- spring-boot-starter-logging
		- logback, slf4j
````

### 톰캣

Spring Framework과는 다르게 Spring boot는 톰캣이 **내장**되어있다. 기존의 Spring 기반 웹 프로젝트만 경험해본 나로써는 내장 톰캣이 어떻게 **WAS**를 대체하는지 궁금증이 생긴다. 이와 관련해 추후에 포스팅할 예정이다.

![스프링부트 라이브러리](https://user-images.githubusercontent.com/49560745/103195983-aa51bb00-4926-11eb-8934-0179c15637d5.png)



## 테스트 라이브러리

Spring Framework과의 차이점이 이 부분에서도 나타났다. 최근 개발방법론의 추세라고 할 수 있는 **TDD**관점에서 Spring boot의 환경이 구축된 것같다. 자동으로 **테스트 라이브러리**가 설정되고, 테스팅 소스폴더가 default로 존재했다.

```
spring-boot-starter-test 
	- junit: 테스트 프레임워크 
	- mockito: 목 라이브러리 
	- assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리 
	- spring-test: 스프링 통합 테스트 지원
```

![스프링부트 라이브러리](https://user-images.githubusercontent.com/49560745/103196261-2c41e400-4927-11eb-8fcf-95eb6570d7de.png)

System.out.print()는 현업에서 사용하지 않는다. log로 따로 남겨야 log파일이 관리되고, **심각한 오류**를 정리할 수 있다.



### Junit 테스트 라이브러리

Junit은 자바 프로그래밍 언어용 **유닛 테스트 프레임워크**이다. Junit은 테스트 주도 개발에서 중요하다. 이번 기회에 Junit을 프로젝트에 적극 적용해 볼 예정이다.

![스프링부트 라이브러리](https://user-images.githubusercontent.com/49560745/103196316-52678400-4927-11eb-982a-29d461ac995e.png)



<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49571?tab=curriculum&q=107977)

