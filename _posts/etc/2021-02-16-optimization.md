---
title: Web 성능최적화 - FE
toc: true
categories:	
    - etc
tags: 
- 애플리케이션 최적화
last_modified_at:
---

# 로딩 최적화

- 브라우저 기준 최적화의 문제점
- 사용자 기준 최적화
- 프리 렌더러
- PWA 사례



브라우저 로딩 과정

```
parse -> style -> layout(글자하나하나 위치 지정하고 구조를 짜야해서 비용이크다) -> composite & render -> paint
```



```
HTML 파싱 =>DOM 트리
CSS 파싱 => CSSOM 트리

두개를 합쳐서 => Render 트리
```



css , js는 블록 리소스

## 블록리소스 최적화

자바스크립트 로드 시점 최적화

- `</body>` 태그 하단에 위치시켜라
- `<head>`태그에 뒤되 `async`나 `defer`를 사용하라
  - `async`나 `defer`를 쓰면 블록을 만들지 않고(끊기지않고) 렌더링을 계속 한다.



css 최적화

- 외부스타일 => 인라인 스타일 (wating time을 줄일 수 있다.)



사용자 기준 최적화가 중요하다.

- 사용자 입장에서 중요한 컨텐츠를 먼저 보여줘야한다.
- `First Meaningful Paint`



`SPA` -> `SSR`



서버 사이드 렌더링

- 생성시점 : 런타임

프리렌더러

- 서버 사이드 렌더링 종류 중 하나

- 생성시점 : 빌드타임



로딩 성능 최적화 : PWA

- PWA = Web + App

  



# 렌더링 최적화

- 레이아웃 스래싱

레이아웃 성능이 떨어졌다면, **강제 동기 레이아웃**이 발생했는지 확인해보기

**강제 동기 레이아웃**

`javascript`에서 DOM변경, CSS변경은 필연적으로 일어난다.

이때, 브라우저 로딩 과정이 그래도 반복된다.



레이아웃 스래싱



강제 동기 레이아웃 개선

=> 캐싱하기

- 가상돔

가상돔은 불필요한 돔 변경을 피할 수 있다.

VIEW의 `snabbdom` => `h` 함수 => 불필요한 업데이트를 줄일 수 있다.

- 웹워커

60 FPS = 16ms/fr => 10ms/fr

1초에 60 Frame = 60 FPS => 가장 적합한 프레임 속도



무거운 작업 / 오래 걸리는 프레임

=> 웹워커에 작업을 넘겨라

웹팩 설정

```
로딩 최적화 : 브라우저 기준(DomContentLoaded, load)
로딩 최적화 : 사용자 기준(Critical Rendering Path,FMP,SSR)
로딩 최적화 : Progressive Web App 사례
렌더링 최적화 : 레이아웃 스래싱
렌더링 최적화 : 가상돔
렌더링 최적화 : 웹 워커
```



<br/>

# Reference

- [TOAST - 프런트엔드 성능 최적화 ](https://www.youtube.com/watch?v=G1IWq2blu8c)
