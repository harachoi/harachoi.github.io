---
title: Database 기술 면접 정리
toc: true
categories:	
    - Interview
tags:
- Database
last_modified_at: 
---



`Daily Update`

## Database

### JDBC

- 자바에서 DB의 종류에 상관없이 데이터베이스에 쉽게 접근할 수 있도록 하는 API

### DBCP

- `DataBase Connection Pool`의 약자
- 애플리케이션에서 `DB`로 요청할 때, 가장 많은 비용이 소모되는 **커넥션 객체 생성** 비용을 줄일 수 있다.
- `Pool` 에 미리 `Connection` 객체를 생성해두고, 필요할 때마다 가져와 사용하고 반납하는 구조이다.
- `Tomcat(서버)`의 스레드 하나 당 하나의 커넥션 객체가 일대일로 매칭된다.
  - 커넥션 객체를 모두 사용하고 있을 때, 요청이 들어오면 스레드는 대기열에서 대기한다.
  - `Tomcat(서버)` 스레드 풀의 스레드가 모두 소진되면 `Tomcat` 은 오류를 뱉는다.
    - Q. 커넥션 객체를 가능한 많이 만들어 놓으면?
    - A. 일반적으로 `DBMS`의 리소스는 다른 서비스와 공유해 사용하는 경우가 많기에 무조건 커넥션 개수를 늘리는게 답이아니다.
  - 사용자가 인내할 수 없는 `MaxWait` 값은 쓸모 없다.
  - 그렇다고 `MaxWait`를 너무 작게 설정하면 과부하 시 커넥션 풀에 여분의 객체가 없을 때 너무 자주 오류 메시지를 보게된다.

#### Ref.

[naver D2 - DBCP](https://d2.naver.com/helloworld/5102792)

### statement preparation

- `RDBMS` 실행에 있어 비용이 많이 드는 작업이며, 복잡한 `SQL`일 수록 더욱 높은 비용 소모

```
문법 오류 검증
컬럼 참조의 유효성 검증
최적화 된 접근경로
execution plan의 식별
```



### prepared statement pool

- 자주 사용되는 `statement` 를 사전에 `prepare` 하여 `pool`에 저장해놓는 것
- `pool`에서 `Prepare`된 statement을 갖고 오는 방법을 Prepared Statement Pooling
- statement preparation에 소모되는 비용을 최소화 할 수 있음

#### 생성 방법

- `connection pool` 이 있다면 `pool`에서 connection을 얻어 온다.
- `preparedstatement pool` 생성하기

![image](https://user-images.githubusercontent.com/49560745/108307551-501bfa80-71f1-11eb-8c6d-fda40c5f6bf7.png)



#### 사용하는 이유

- `RDBMS`가 `statement`를 준비하는 시간을 줄여주기때문에 애플리케이션이 `RDBMS`로부터 데이터를 로딩하는 시간이 향상됨
- `Statement caching`이 자동으로 수행됨
- `Statement pooling`과 `Connection pooling`을 함께 사용하면 `Connection pooling`을 단독으로 사용하는 것보다 큰 성능향상을 가져옴

#### connection pool과 preparedstatement pool을 같이 사용했을 때 장점

- 커넥션 풀을 사용하지 않으면 애플리케이션이 `disconnect` 되었을 때 `prepared statement pool`이 사라진다.
- 하지만, 커넥션 풀을 사용한다면 `prepared statement pool`이 사라지지 않고,`disconnect` 될 때 `connection pool`과 `prepared statement pool`이 함께 return 된다.
- 이후에 애플리케이션이 `connect` 했을 때 `Statement prepare`를 하지않고, `prepared statement pool`에 있는 `prepared statement`를 재사용할 수 있다.

#### Ref.

[prepared statement pool](https://zzikjh.tistory.com/entry/DBCP-%EC%82%AC%EC%9A%A9%EC%8B%9C-poolPreparedStatements-%EC%86%8D%EC%84%B1%EC%9D%B4-%EC%84%B1%EB%8A%A5%EC%97%90-%EB%AF%B8%EC%B9%98%EB%8A%94-%EC%98%81%ED%96%A5)

### statement

- `SQL` 구문을 실행하는 역할
- 구문 실행 후 결과를 반환하는 역할



### perpared statement vs statement

**[참고]**  `SQL` 실행과정

```
1) 쿼리 문장 분석
2) 컴파일
3) 실행
```

- 둘의 가장 큰 차이점은 `캐싱` 사용 여부이다.

- `statement`는 쿼리를 실행할 때 마다 1) ~ 3)의 과정을 거친다
- `prepared statement`는 컴파일을 미리 준비해두기 때문에 `statement`보다 성능상 이점이있다.
- 동일한 쿼리에서는 `prepared statement` 유리
  - select * from a    => 10만건이상의 데이터 가져오기
- 조건절이 매번 바뀌는 쿼리는 `statement` 유리



### 인덱스에는 왜 Hashtable 보다 B Tree 인가?

`Hashtable`의 `search` 시간복잡도는 `O(1)`이다. 그 어떤 자료구조보다 검색을 할 때 빠르다. 그럼에도 불구하고 `Database`의 `index`의 구조로 왜 `B Tree`를 선택했을까?

그 이유는 바로 데이터베이스의 데이터 구조 특성때문이다. 

데이터베이스에서 자료를 **조회**할 떄 특정 데이터만을 추출하기도 하지만, 범위 설정이 필요한 경우가 많다.

예를들어, 키가 180이상인 사람의 데이터를 추출하고 싶다면 조건문에 부등호 `>=`  기호를 사용해야한다. 이 부등호는 `Hashtable`에서는 사용할 수 없다. `Hashtable`은 `{key : vlaue}` 쌍의 구조로 특정 데이터를 가리키는 `key` 값만 가지고 있을 뿐 부등호 연산을 할 수 없는 것이다. 그렇기 때문에 부등호 연산에 적합한 `B Tree`를 사용한다. 조회 시간복잡도도 `log(n)`으로 준수하다.

### 인덱스 성능 고려할 점

`SELECT` 쿼리의 성능을 향상시키는 `INDEX`는 항상 좋지만은 않다.

- `INSERT`의 경우 `INDEX`에 대한 데이터를 추가해야하므로 그만큼 성능에 손실이있다.
- `DELETE`의 경우 `INDEX`에 존재하는 값을 삭제하는 것이 아닌 사용하지 않는다는 표시가 남는다.
  - 즉, row 수는 그대로이다. 실제 데이터는 1000건인데 데이터가 10만이되는 결과를 낳을 수 있다.
- `UPDATE`의 경우 `INSERT`, `DELETE` 두 경우의 문제점을 모두 수반한다.
  - `UPDATE`는 이전 데이터를 삭제하고, 새 데이터를 삽입하는 개념이기 때문이다.

### 인덱스 활용법

- 전체 데이터 중 10~15% 이내의 데이터를 검색할 때 사용하면 효율적이다.
- 두 개이상의 컬럼이 `WHERE`절이나 `JOIN` 조건으로 자주 사용되는 경우 인덱스를 생성하는 것이 효율적이다.
- 한 테이블에 저장된 데이터 용량이 상당히 클 경우 유리하다.
- 가능 한 수정이 빈번하게 일어나지 않는 컬럼을 설정한다.

### 트랜잭션 격리 수준(isolation)



### UNDO, REDO

- `UNDO`는 실행취소, `REDO`는 다시하다 라는 뜻을 갖는다.

- `REDO`와 `UNDO`의 공통점은 복구이다.
  - `REDO`는 복구할 때, 사용자가 했던 작업을 그대로 다시한다.
  - `UNDO`는 복구할 떄, 사용자가 했던 작업을 반대로 한다. 즉, 작업을 원상태로 되돌린다.

#### Ref.

https://brownbears.tistory.com/181

### Primary key , Foreign key

- `Primary key` : 테이블에서 각 행(row)를 유일하게 구분하는 key
- `Foreign key` : 하나의 테이블에 있는 열(column)으로는 그 의미를 표현할 수 없는 경우, 다른 테이블의 `Primary key` 열(column)을 반드시 참조해야하는 key

### NoSQL(ex, MongoDB)

#### 개념

- `Not Only SQL` 의 약자
- 분산 환경에서 대용량의 데이터를 빠르게 처리하기 위해 개발
  - 빅데이터를 `RDBMS`의 스키마에 맞춰 변경해 넣으면 매우 긴 시간의 **down time**이 발생
  - 관계모델, 트랜잭션 연산, 일관성, 속성을 유지하면서 분산환경에서 `RDBMS`를 조작하기 힘듦
- 릴레이션이 아니므로 고정된 스키마가 없고, 조인이 힘듦

#### 특징

- 거대한 `Map` 형태로 `key : value` 형식을 지원
- 관계를 정의하지 않음
- 대용량 데이터 저장
- 분산형 구조를 통해 데이터의 백업본을 만들어 데이터 유실이나 서비스 중지에 대비
- 읽기보다 쓰기 작업이 빠름

### MySQL

- 오픈소스 관계형 데이터베이스
- 다양한 운영체제에서 사용 가능