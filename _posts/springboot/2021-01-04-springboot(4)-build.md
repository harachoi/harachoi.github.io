---
title: Spring boot기반 Web Application 개발[4] - 빌드하기
toc: true
categories:	
    - Spring boot
tags:
- Spring boot
- Spring Web
last_modified_at:
---



Spring boot를 활용해 프로젝트를 빌드하는 방법에 대해 알아보자.

# Build란?

```
컴퓨터 소프트웨어 분야에서 소프트웨어 빌드는 소스 코드 파일을 
컴퓨터나 휴대폰에서 실행할 수 있는 독립 소프트웨어 가공물로 
변환하는 과정을 말하거나 그에 대한 결과물을 일컫는다. 
출처 - 위키백과
```

Build란 소스 코드파일을 컴퓨터 혹은 휴대폰에서 실행할 수 있도록 독립적인 형태로 변환하는 과정이라고 할 수 있다. Spring boot에서 Build를 하는 방법은 두 가지가 있다.

1) IDEA(IntelliJ)내에서 Build하기

2) cmd(혹은 Git bash)로 Build하기

- cmd에서 Build가 필요한 이유는 "실제 운영환경에서 소스코드를 서버에 배포해야 하는데 이때는 IDE 없이 소스코드를 빌드할 수 있어야 하기"때문이다.



두 방법 모두 실행하는 방법은 동일하다. 그 전에 환경 설정이 필요하다

bash를 실행할 수 있는 환경을 만들어주는 방법은

1) [IntelliJ & Gitbash 연동하기](https://gwang920.github.io/intellij/intellij&gitbash/)

2) [cmd - bash 환경설정](https://devms.tistory.com/58)

위 링크를 참고하자.

<br/>

# Build 하기

 IDEA 혹은 cmd에서 build하는 과정은 다음과 같다.(포스팅은 Gitbash 기준)

우선, 프로젝트 폴더로 이동하자.

 command에서 프로젝트 폴더로 이동

![image](https://user-images.githubusercontent.com/49560745/103506005-10968a80-4e9f-11eb-92fe-c1ead0873b1d.png)

기본 폴더구조는 아래와 같다. `ls` 명령어로 확인할 수 있다.

![image](https://user-images.githubusercontent.com/49560745/103506221-8995e200-4e9f-11eb-9bf9-366a372d7b69.png)

<br/>

이제, Build 해보자. 순서는 다음과 같다.

```
1) ./gradlew build
2) cd build/libs
3) java -jar hello-spring-0.0.1-SNAPSHOT.jar
4) 실행확인
```

<br/>

1 - **./gradlew build** 입력

![image](https://user-images.githubusercontent.com/49560745/103510191-0d53cc80-4ea8-11eb-8682-f897bd7be985.png)

**BUILD SUCCESSFUL**이라는 메시지가 뜨면, 성공적으로 build가 된것이다.

<br/>

![image](https://user-images.githubusercontent.com/49560745/103506195-78e56c00-4e9f-11eb-89cb-f51aba82d958.png)

`/build`로 이동 후 `ls`를 입력하면 build 폴더에 **libs** 폴더가 생성되어있음을 확인할 수 있다.

(libs 폴더가 생성되지 않는 경우가 있다. 아래 오류 사항을 참고해 PC와 IDE의 jdk 버전을 확인하고, 설정해주자.)

<br/>

![스프링부트 github에서 빌드하기 성공](https://user-images.githubusercontent.com/49560745/103508194-f7dca380-4ea3-11eb-86a0-6d4878469824.png)

2 - **cd libs**로 이동한 후

3 - **java -jar hello-spring-0.0.1-SNAPSHOT.jar** 명령어를 실행하여 빌드해보자.

4 - 위와 같은 화면이 출력되면 성공이다!

<br/>

# 오류

**1) Version Error**

```
Exception in thread "main" java.lang.UnsupportedClassVersionError: hello/hellospring/HelloSpringApplication has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0

        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(Unknown Source)
        at java.security.SecureClassLoader.defineClass(Unknown Source)
        at java.net.URLClassLoader.defineClass(Unknown Source)
        at java.net.URLClassLoader.access$100(Unknown Source)
        at java.net.URLClassLoader$1.run(Unknown Source)
        at java.net.URLClassLoader$1.run(Unknown Source)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(Unknown Source)
        at java.lang.ClassLoader.loadClass(Unknown Source)
        at org.springframework.boot.loader.LaunchedURLClassLoader.loadClass(LaunchedURLClassLoader.java:151)
        at java.lang.ClassLoader.loadClass(Unknown Source)
        at java.lang.Class.forName0(Native Method)
        at java.lang.Class.forName(Unknown Source)
        at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:46)
        at org.springframework.boot.loader.Launcher.launch(Launcher.java:109)
        at org.springframework.boot.loader.Launcher.launch(Launcher.java:58)
        at org.springframework.boot.loader.JarLauncher.main(JarLauncher.java:88)
```

**55 = Java 11 버전**

**52 = Java 8 버전**

"has been compiled by a more recent version of the Java Runtime (class file version 55.0)

this version of the Java Runtime only recognizes class file versions up to 52.0" 

메시지를 자세히보면, 로컬 PC에서 자바 버전이 8로 운영되고 있기 때문에 **버전 에러**가 발생하는 것이다.



**해결방법**

1 - 환경변수 변경하기

```
Windows 10 기준 환경 변수 설정하기

sysdm.cpl 입력 => 고급 => 환경 변수

1) User에 대한 사용자 변수
- Path => 편집 => 새로만들기 => "C:\Program Files\Java\jdk-11.0.9\bin" 추가

2) 시스템 변수
- 편집 => C:\Program Files\Java\jdk-11.0.9\bin 추가
- 새로 만들기 => 변수이름 : JAVA_HOME , 변수 값 : C:\Program Files\Java\jdk-11.0.9

```

2 - 기존 Jdk 버전 삭제 하기

````
프로그램 추가/제거 => java 기존 버전 삭제
````

3 - 버전 확인하기

```
cmd(명령 프롬프트)
1) java -version
2) javac -version

현 프로젝트 기준 아래와 같이 나오면 성공이다.

C:\Users\User>java -version
java version "11.0.9" 2020-10-20 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.9+7-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.9+7-LTS, mixed mode)

C:\Users\User>javac -version
javac 11.0.9
```

4 - IntelliJ 내 환경 설정

```
1. [File] - [Project탭]
```

Project SDK에 11.0.9 version으로 맞춰놓고 11.0.9  version의 jdk의 경로를 설정해준다.

```
2. [File] - [Setting] - [Build, Execution, Deployment 탭] - [Gradle 탭]
```

Gralde JVM - 1.8 version으로 설정되어 있는 것을 11.0.9 version으로 변경해준다.

<br/>

이 포스팅은 인프런 김영한님의 `스프링 입문 - 코드로 배우는 스프링 부트 강의`를 토대로 작성되었습니다.

# Reference

- [김영한님의 스프링-입문-스프링부트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/48553?tab=curriculum&q=107977)
- [김영한님의 스프링-입문-스프링부트:Question](https://www.inflearn.com/questions/53693)
- [김영한님의 스프링-입문-스프링부트:Question](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/lecture/49574?tab=question&q=82121)

