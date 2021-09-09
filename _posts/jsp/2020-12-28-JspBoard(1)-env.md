---
title: JSP 게시판 제작[1] - 환경구축
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
ECIPSE
 JAVA(JDK-11.0.9)
 bootstrap-3.3.7
 MY SQL 8.0
 Tomcat 8.5
```

## 1) 부트스트랩 설정

```
부트스트랩을 다운 받아 js폴더로 이동
(아래 JSP는 임의로 만든 프로젝트 폴더)

C:\JSP\bootstrap-3.3.7-dist\js
해당 경로에 있는 3개 파일을 paste해준다.
```

[![image](https://user-images.githubusercontent.com/49560745/98509587-cfba3500-22a4-11eb-8f39-5d698942d441.png)](https://user-images.githubusercontent.com/49560745/98509587-cfba3500-22a4-11eb-8f39-5d698942d441.png)

## 2) java Build Path

```
프로젝트 우클릭 => Build Path => configuer Build Path..
----------------
mysql connector
tomcat
jdk
----------------
추가
```

[![image](https://user-images.githubusercontent.com/49560745/98509722-0ee88600-22a5-11eb-9207-7be69aa70ea2.png)](https://user-images.githubusercontent.com/49560745/98509722-0ee88600-22a5-11eb-9207-7be69aa70ea2.png)

# Reference

- https://www.youtube.com/playlist?list=PLRx0vPvlEmdAZv_okJzox5wj2gG_fNh_6