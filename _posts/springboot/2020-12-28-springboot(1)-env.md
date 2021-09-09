---
title: Spring boot기반 Web Application 개발[1] - 환경구축
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at: 
---

 Spring boot 기반의 개발환경을 구축해보자. Java 버전은 11, IDE는 IntelliJ를 사용한다.

# 1. Spring boot 다운로드

https://start.spring.io/ 에 접속해 Spring boot를 다운로드하자.

![spring initializr](https://user-images.githubusercontent.com/49560745/103190612-11ff0a80-4915-11eb-9d11-588bd9af497a.png)

**SNAPSHOT**이나 **M1**은 아직 정식버전이 아니라고한다. 버전은 **2.3.6**을 설치했다. Group명과 Artifact명은 자유롭게 설정하자. 나머지 Default로 두면된다.



![Dependencies](https://user-images.githubusercontent.com/49560745/103190703-75893800-4915-11eb-9388-4564f6a9aee5.png)



우측에는 **ADD DEPENDENCIES**로 원하는 개발환경을 구축할 수 있다. Web 개발이기 때문에 **Spring Web**과**Thymeleaf(HTML을 만들어주는 템플릿엔진)**를 추가해준다. 그리고 하단에 **GENERATE**를 클릭하면 압축파일이 다운로드된다.

# 2. 프로젝트 Import

![IntelliJ ](https://user-images.githubusercontent.com/49560745/103190944-563eda80-4916-11eb-98cd-9c6502e0a8fe.png)

**1) Open or Import**<br/>
**2) hello-spring**<br/>
**3) build.gradle**<br/>
**4) open as project**

다운로드받은 파일을 압축 해제하고, 위 순서에 따라 import 해준다. import를 하고 난 뒤 자동으로 설치가 될 때까지 기다려준다.



![image](https://user-images.githubusercontent.com/49560745/103191102-e3822f00-4916-11eb-9c81-74dbff20095b.png)



기본 디렉토리 구조는 위와 같다. java 파일을 제외하고 HTML과 같은 파일들은 전부 resources에 저장된다는 것을 기억하자.

이제, 프로젝트를 **run** 시켜보자. (실행 시 애먹었던 오류는 따로 아래 정리해두었다.)

# 3. page 로드

![image](https://user-images.githubusercontent.com/49560745/103193608-4b894300-4920-11eb-80f6-d181585155b2.png)

설정한 포트번호 (Default는 8080)로 접속 시 위와 같은 page가 로드되면 **성공**이다!

<br/>

# 오류

**1) Web server failed to start. Port 8080 was already in use 에러**

![Port 8080 was already in use error](https://user-images.githubusercontent.com/49560745/103193546-1c72d180-4920-11eb-9d2e-21660425572d.png)

default는 **8080** 포트로 설정되어있다.

포트번호가 겹칠경우 , **src -> main -> resources -> application.properties**로 이동 후  포트번호를 변경해주자.



**2) Process 'command 'C:/Program Files/Java/jdk1.8.0_261/bin/java.exe'' finished with non-zero exit value 1**

JDK 버전이 일치하지 않아 발생하는 오류이다. 아래 링크를 따라 해결할 수 있다.

- https://www.inflearn.com/questions/69450

- https://twofootdog.tistory.com/57

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/48553?tab=curriculum&q=107977)