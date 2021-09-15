---
title: VaxTrax
excerpt: See the progress of Covid-19 Vaccination
header:
  image: /assets/images/board.jpg
  teaser: /assets/images/board.jpg
sidebar:
  - title: "담당 역할"
    text: "Front-End & Back-End"
  - title: "4인 팀 프로젝트"
gallery:
  - url: /assets/images/Portfolio/covid19-1(cut).jpg
    image_path: assets/images/Portfolio/covid19-1(cut).jpg
    alt: "placeholder image 1"
  - url: /assets/images/Portfolio/covid19-2(cut).jpg
    image_path: assets/images/Portfolio/covid19-2(cut).jpg
    alt: "placeholder image 2"
  - url: /assets/images/Portfolio/covid19-3(cut).jpg
    image_path: assets/images/Portfolio/covid19-3(cut).jpg
    alt: "placeholder image 3"
  - url: /assets/images/Portfolio/covid19-4(cut).jpg
    image_path: assets/images/Portfolio/covid19-4(cut).jpg
    alt: "placeholder image 4"
layout: archive
classes: layout--home
---

## Project Obejctives

  - Develop a web app that graphically displays COVID-19 vaccination data along with other useful COVID-19 related statistics.
  - Display vaccinations administered.
  - Display state vaccination distribution.
  - Predict vaccine doses based on current distribution.
  - Display a herd immunity progress bar, the percent of people vaccinated, for each U.S. state.
  - Identify states that are struggling to administer the vaccine through color coordination.

## 1. Non-Functional Issues

```
Architecture: Client-Server
Language: Python
Frontend Framework: Plotly Dash/Flask
Data visualization/Querying API: Plotly
CSV data loaded from: AWS S3 Urls & Data Migration Script
Service: AWS Elastic Beanstalk
```

## Design Outline

![vaxtrax_flow](https://user-images.githubusercontent.com/29204186/133462137-2df48823-f58f-48c3-ae4f-e40341ecffd5.JPG)

<!-- <img src="https://github.com/harachoi/harachoi.github.io/tree/master/assets/images/VaxTrax/vaxtrax_flow.PNG" width="100%" align="center"> -->


<!-- ## 기능 개요

- 회원가입, 로그인 기능
- 게시판 기능
- 댓글 기능
- 사용자 블락 및 팔로우 기능
- 사용자 간의 메세지 보내기 기능 -->

## 화면 구성                                                                                                                                                                              

프로젝트 화면 구성 Site 맵 - 이미지를 클릭해주세요!

{% include gallery caption="" %}


## Code Details
## [Github-Source Code](https://github.com/harachoi/VaxTrax)

