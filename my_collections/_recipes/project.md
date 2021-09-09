---
title: Spring Framework 기반의 메뉴추천시스템
permalink: /project/
toc: true
project:	
    - Spring Framework 기반의 메뉴추천시스템
tags:
- Spring
last_modified_at:
---



## 1. Spring Framework 기반의 메뉴추천시스템

### I. 주제 및 기획의도

- 주제 : Spring Framework기반 메뉴추천 시스템
- 기획의도 : 식사 시간마다 고민되는 메뉴 선택을 돕는 웹서비스 개발

### II. 기능 개요

- 회원가입을 통해 사용자를 인식하여 관심 메뉴 / 기피 메뉴 선택권한을 이용자에게 부여
- 위치 검색 기능을 도입하여 룰렛에서 선정 된 메뉴에 대한 식당 추천
- 댓글 기능, 별점 평점 기능, 5가지 표현 기능 구현

### III. 시스템 구조도 및 시나리오

[![image](https://user-images.githubusercontent.com/49560745/101629531-d9ca8180-3a64-11eb-9787-5cfe40075abd.png)](https://user-images.githubusercontent.com/49560745/101629531-d9ca8180-3a64-11eb-9787-5cfe40075abd.png)

[![image](https://user-images.githubusercontent.com/49560745/102046855-861eb600-3e1f-11eb-91fa-54911180a94c.png)](https://user-images.githubusercontent.com/49560745/102046855-861eb600-3e1f-11eb-91fa-54911180a94c.png)

1. 회원가입/로그인
2. 메뉴 선정(선호메뉴/비선호메뉴 선택 가능)
3. 선택된 메뉴 데이터 룰렛 적재
4. 룰렛 돌리기(최종 메뉴 선정)
5. 선정메뉴 + 검색(사용자가 원하는 위치) = 식당 리스트업
6. 해당 식당 별점 및 리뷰



### IIII. 개발 환경

[![image](https://user-images.githubusercontent.com/49560745/101629583-efd84200-3a64-11eb-9793-8a3312ee1664.png)](https://user-images.githubusercontent.com/49560745/101629583-efd84200-3a64-11eb-9793-8a3312ee1664.png)

### IV. ERD 설계

[![image](https://user-images.githubusercontent.com/49560745/101631109-2dd66580-3a67-11eb-8ee2-5a6c955b6d58.png)](https://user-images.githubusercontent.com/49560745/101631109-2dd66580-3a67-11eb-8ee2-5a6c955b6d58.png)

### V. 화면 설계

#### main - 네비게이션바

[![image](https://user-images.githubusercontent.com/49560745/101631569-e56b7780-3a67-11eb-8841-f1237112aaa9.png)](https://user-images.githubusercontent.com/49560745/101631569-e56b7780-3a67-11eb-8841-f1237112aaa9.png)

#### 회원가입

[![image](https://user-images.githubusercontent.com/49560745/101631642-046a0980-3a68-11eb-86f7-a9970b52fa7a.png)](https://user-images.githubusercontent.com/49560745/101631642-046a0980-3a68-11eb-86f7-a9970b52fa7a.png)

#### 로그인

[![image](https://user-images.githubusercontent.com/49560745/101631612-f4522a00-3a67-11eb-920e-a5bca6ef3af9.png)](https://user-images.githubusercontent.com/49560745/101631612-f4522a00-3a67-11eb-920e-a5bca6ef3af9.png)

#### 메뉴추가

[![image](https://user-images.githubusercontent.com/49560745/101631717-26638c00-3a68-11eb-9066-9d6291afc9d6.png)](https://user-images.githubusercontent.com/49560745/101631717-26638c00-3a68-11eb-9066-9d6291afc9d6.png)

#### 룰렛돌리기

[![image](https://user-images.githubusercontent.com/49560745/101631860-63c81980-3a68-11eb-820b-9bd412e98bb2.png)](https://user-images.githubusercontent.com/49560745/101631860-63c81980-3a68-11eb-820b-9bd412e98bb2.png)

#### 위치 검색

[![image](https://user-images.githubusercontent.com/49560745/101631896-74788f80-3a68-11eb-9260-2cacc51facf3.png)](https://user-images.githubusercontent.com/49560745/101631896-74788f80-3a68-11eb-9260-2cacc51facf3.png)

#### 리뷰 및 댓글 표현 기능

[![image](https://user-images.githubusercontent.com/49560745/101631953-865a3280-3a68-11eb-9814-5251fb7777da.png)](https://user-images.githubusercontent.com/49560745/101631953-865a3280-3a68-11eb-9814-5251fb7777da.png)



#### 소스코드

https://github.com/gwang920/MenuRecommandationSystemProject