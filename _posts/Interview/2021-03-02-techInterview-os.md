---
title: OS 기술 면접 정리
toc: true
categories:	
    - Interview
tags:
- OS
last_modified_at: 
---



`Daily Update`

## OS

## 운영체제란?

- 운영체제는 
  - 컴퓨터 시스템의 **자원들을 효율적으로 관리**하고,
  - 사용자가 컴퓨터를 편리하고 **효과적으로 사용**할 수 있도록 **환경을 제공하는 여러 프로그램의 모임**

## 커널

### 커널이란?

- 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하여  응용프로그램이 하드웨어 자원을 관리하고, 사용할 수 있게 해주는 것
- `Application` - `Kernel` - `Hardware`
  - `Hardware` : Cpu, Memory, Devices 등

### 커널 기능

- 컴퓨터에 속한 자원들에 대한 **접근을 중재**한다.

- 커널은 운영 체제의 핵심 부분이다.
  - 유저(어플리케이션, 응용프로그램)은 `Shell`을 이용하여 `Kernel`을 통해 하드웨어에 접근하고, 하드웨어의 자원을 사용할 수 있다.
  - `Shell`의 `I/O`요청이 적합한지 판단하고 적합하다면 실행한다. (`I/O` : 입출력)
- 커널은 여러 프로그램 중 어느것이 프로세스에 할당되어야하는지 결정하는 책임이 있다.
  - CPU 스케쥴링

### RAM(Random - Access - Memory)

- 램은 프로그램의 명령어와 데이터를 저장하는대에 이용된다.
- `Kernel`은 각 프로세서가 사용할 수 있는 메모리를 결정하고, 메모리가 부족할 때 수행할 행동을 결정한다.

## IPC

**Inter-Process Communication** : 프로세스 간 통신

- 프로세스들 사이에 서로 데이터를 주고 받는 행위

## Protection

- `Kernel`은 임의의 프로세스가 다른 프로세스의 주소공간에 접근하는 것을 막는다.
  - 이게 바로 **Protection**이다.
- 오직 `Kernel`만이 프로세스의 주소공간에 접근할 수 있다.
  - `Kernel`은 신뢰되는 것이기 때문

### Ref.

[IPC란 ?](http://blog.naver.com/PostView.nhn?blogId=green187&logNo=110130416319&redirect=Dlog&widgetTypeCall=true)

## 윈도우 vs 리눅스

### 윈도우

- 싱글유저 시스템
  - 개인 컴퓨터에서 사용되는 운영체제이다.
  - 보안문제와 큰 관련이 없다.
    - 혼자 사용하니까!
- `GUI`를 제공해 손쉽게 조작할 수 있다.
  - 대신 `Linux`에 비해 더 많은 비용이 소모된다.
- **Protection** : little

### 리눅스

- 멀티유저 시스템
  - 보안
  - 메모리 관리

- `Windows`가 `GUI`를 제공하는 것과 달리 `CUI`를 제공한다.
  - CUI : Commad Line Interface
  - 명령어를 통해 조작한다.
  - `GUI`보다 자원, 비용 소모가 적다. => 빠르다.

### Ref.

[리눅스-커널-운영체제-강의노트](https://medium.com/pocs/%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%BB%A4%EB%84%90-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B0%95%EC%9D%98%EB%85%B8%ED%8A%B8-1-d36d6c961566)