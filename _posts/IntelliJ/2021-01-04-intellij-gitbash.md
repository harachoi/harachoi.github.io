---
title: Intellij & Git Bash 연동하기
toc: true
categories:	
    - IntelliJ
tags:
- intelliJ
- gitbash

last_modified_at: 
---

**Intellij Community**의 기본 터미널은 Windows의 기본 shell인 **cmd**이다. 따라서, bash(bash는 shell을 대체하는 소프트웨어) 명령어를 사용하기 위해 기본 shell을 변경해줘야한다. 이번 포스팅에서는 Windows의 기본 shell을 Git bash로 변경하는 방법에 대해 알아보자.

<br/>

* Shell & Bash 이란?

```
- 리눅스의 Shell은 명령와 프로그램을 실행할 때 사용하는 인터페이스이다.
- Shell은 커널(Kernel)과 사용자간의 다리역할을 한다.
- 커널(Kernel)은 운영체제의 핵심역할을 한다.
	보안, 자원관리 등을 수행한다.
- Bash는 이 Shell을 대체할 수 있는 소프트웨어이다.

즉, Shell은 커널과 소통하기 위한 인터페이스이고, Bash를 사용해
Shell의 기능을 활용할 수 있다.
```



Git Bash는 Github를 사용해봤다면 굉장히 익숙할 것이다. Git Bash를 사용하는 이유는 [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) 명령어를 사용할 수 있기 때문이다. Git Bash 내에선 *nix계열의 OS에서 쓸 수 있는 명령어인 ssh, scp, cat, find 등을 쓸 수 있다.

<br/>

# IntelliJ Terminal, Gitbash로 변경하기 

이제, **IntelliJ**에서  Terminal을 Gitbash로 변경해보자.



우선, Intellij에서 **Crtl + Alt + s** 키를 눌러 Settings 창을 열자.

![image](https://user-images.githubusercontent.com/49560745/103500599-e1781d00-4e8e-11eb-8503-504fad1527fa.png)

위와 같은 창에서 좌측 상단의 검색란에 **Terminal**을 입력하자. 기본 Shell path는 **cmd.exe**로 설정되어있음을 확인할 수 있다. 이제, Shell path를 Git의 **sh.exe**로 변경해주자. 

<br/>

![image](https://user-images.githubusercontent.com/49560745/103500355-63b41180-4e8e-11eb-9143-c2a3dfdad189.png)

````
"C:\Program Files\Git\bin\sh.exe(shell이 설치된 경로)" -login -i
````

 shell이 설치되어있는 경로를 ""로 감싸고, 공백 **-login** 공백 **-i**를 입력하자. 그리고 **Apply**, **OK** 버튼을 누르고 IDEA를 재구동하자.

<br/>

터미널에 접속하면 이제 기본 shell이 **bash**로 변경된 것을 확인할 수 있다!

![image](https://user-images.githubusercontent.com/49560745/103500867-c78b0a00-4e8f-11eb-8101-8fd626088be4.png)

**[변경 전]**

![image](https://user-images.githubusercontent.com/49560745/103501125-a4ad2580-4e90-11eb-8e1b-0454ade69c87.png)

**[변경 후]**

<br/>

# Reference

- [IntelliJ IDEA와 Git Bash 연동하기](https://violetboralee.medium.com/intellij-idea%EC%99%80-git-bash-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0-63e8216aa7de)

- [셸이란? - 양햄찌가 만드는 세상](https://jhnyang.tistory.com/57)