---
title: Clustered vs NonClustered (index 개념)
toc: true
categories:	
    - Database
tags:
- 데이터베이스
- Clustered 
- NonClustered 
last_modified_at: 
---

 

 이번 포스팅에서는 `Database`의 `index` 와 관련된 **Clustered**와 **NonClustered**의 개념을 알아보자.



##  인덱스(Index)란 ?

```
데이터베이스 분야에서 테이블에 대한 동작 속도를 높여주는 자료구조
```

![index](https://user-images.githubusercontent.com/49560745/107173438-200f7300-6a0b-11eb-88f7-4d7fc78a8c3c.png)

`Database`에서 말하는 `index`는 일반적인 책에서 말하는 **목차**에 비유할 수 있다. 만약 책에서 특정 챕터를 찾아보고 싶다면, 책의 목차(index)를 찾고 목차에 적힌 페이지로 이동하면된다. `index`도 마찬가지이다. 데이터의 **위치를 가리키는 지표**와 같은 것이다. 따라서, 빠른 시간내에 원하는 자료를 찾을 수 있다는 장점이있다. 

이러한 `index`에는 테이블에 있는 **하나 이상의 열로 작성되는 키**가 포함된다. 그리고 이 키값은 `SQL SERVER`에서 `B-Tree` 에 저장된다.(`B-Tree`는 키 값과 연결된 행을 빠르게 찾을 수 있는 자료구조이다.)

`Database`에서 `index`의 구조는 크게 `Clustered` 와 `NonClustered`로 나뉜다. 이제 이 두 가지 **타입**의 개념에 대해 살펴보자.



## Clustered

```
Cluster : 군집
Clustered : 군집화
Clustered Index : 군집화 된 인덱스
```

**클러스터 형** 인덱스는 데이터가 테이블에 물리적으로 저장 되는 순서를 정의(설정)한다.  즉, **클러스터 형** 인덱스는 특정 컬럼을 기준으로 데이터들을 정렬시킨다. 아래 테이블은 `id`가 **클러스터 형** 인덱스로 지정되었으며, `id` 값을 기준으로 데이터들이 정렬되어있는 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/49560745/107177426-2c98c900-6a15-11eb-9c47-087fc3212da1.png)

테이블 데이터는 오직 한 가지의 방법으로만 정렬되기 때문에 오직 테이블 당 하나의 **클러스터 형** 인덱스만 존재할 수 있다. 즉, 정렬 기준으로 오직 하나의 컬럼만을 선택할 수 있다. 

`Database`에서 `primary key`의 제약조건(constraint)은 **클러스터 된** 인덱스를 자동으로 생성하기 때문에 우리가 일반적으로 테이블을 생성할 때 특정 컬럼에 `primary key`를 지정했다면, 자료가 자동으로 정렬되는 것이다. 

`primary key`를 설정하지 않은 테이블에 무작위로  데이터를 `insert`하고, 테이블을 조회하면 뒤죽박죽인 테이블을 보게될 것이다.

### 그래서 왜 **Clustered** 인가 ?

위 테이블에 `Name : 넬 , Birth : 1980` 데이터를 `ID : 2 ` 에 추가한다면, 테이블의 상태는 아래와 같을 것이다. 

![image](https://user-images.githubusercontent.com/49560745/107177819-150e1000-6a16-11eb-8792-0ede78e71ecb.png)

이것이 바로 **Clustered**이다. 순서는 오직 하나의 컬럼으로 결정되기 때문에 중간에 새로운 데이터가 삽입된다면 이후의 모든 컬럼을 한 칸씩 이동시켜줘야한다. `index`가 **군집화** 되어있기 때문이다. 

만약, `id : 2` 이후에 만개 혹은 그 이상의 데이터가 있다고 생각해보자. `Insert` 에 소모되는 비용이 어마어마할 것이다. 그렇기 때문에 `Primary Key` 값을 어떤 컬럼으로 선택하는가에 따라 `Database`의 성능이 좌우된다.  

이처럼 `Clustered index`는 검색 속도를 향상시킨다는 장점이 있지만, 새로운 데이터를 삽입할 때는 많은 비용이 소모되는 단점 또한 존재한다.

### Clustered 인덱스의 구조

![clustered Index Structure](https://user-images.githubusercontent.com/49560745/107186321-6246ad80-6a27-11eb-9025-9714c451189e.png)

**Clustered** 인덱스의 구조이다. **Data Page** 의 데이터들이 순차적으로 정렬되어있다. 그리고 Leaf level 과 DataPage가 동일한 구조를 갖는다.

<br/>

### 어떤 경우에 생성해야할까?

- 테이블 데이터가 자주 업데이트 되지 않는 경우
- 항상 정렬 된 방식으로 데이터를 반환해야하는 경우
  - 테이블은 정렬되어있기 때문에 `ORDER BY` 절을 활용해 모든 테이블 데이터를 스캔하지 않고 원하는 데이터를 조회할 수 있다.
- 읽기 작업이 월등히 많은 경우, 이때 매우 빠르다.

<br/>

**[참고]** **Clustered**는 `pk` 값으로 자동 생성되는 것과 별개로 설정을 통해 테이블내에서 원하는대로 생성할 수 있다.

예를들어, 위 테이블을 `Birth`를 기준으로 `Clustered` 인덱스를 생성한다면 결과는 아래와 같다.

![image](https://user-images.githubusercontent.com/49560745/107182509-3b38ad80-6a20-11eb-9bc1-510a147c0836.png)



## NonClustered

```
NonCluster : 비 군집
NonClustered : 비 군집화
NonClustered Index: 군집화되어 있지 않은 인덱스
```

**논 클러스터 형** 인덱스는 말 그대로 **클러스터 형**의 반대인 군집화 되어있지 않은 인덱스 타입이다. 그렇기 때문에 테이블에 저장 된 물리적인 순서에 따라 데이터를 정렬하지 않는다. 즉, 순서대로 정렬되어 있지 않다.

![textbook index](https://user-images.githubusercontent.com/49560745/107176299-4f75ae00-6a12-11eb-9a86-ee82b1a3df9c.png)

그리고 **논 클러스터 형** 인덱스는 테이블 데이터와 함께 테이블에 저장되는 것이 아니라 **별도의 장소**에 저장된다. 마치, 위 책에서 `index` 페이지를 따로 나눈것 처럼 말이다. 그뿐만 아니라 하나의 테이블에 여러개의 **논 클러스터 형** 인덱스를 설정할 수 있다.

**논 클러스터 형**의 예시는 아래와 같다.  **NonClustered** 인덱스는 데이터의 행에 독립적이며, 인덱스 **키 값**과 데이터 행을 가리키는 **포인터**가 존재한다. 

![image](https://user-images.githubusercontent.com/49560745/107178655-f446ba00-6a17-11eb-8f5d-06217ad3d901.png)

테이블의 ID **키 값**과 **포인터**인 Address를 통해 실제 데이터에 접근한다.

`ID : 4` 에 해당하는 가수의 이름을 알고 싶다면, `120`번지로 이동하고, `Name`을 확인하면 된다. 그림에서 볼 수 있듯이 `Clustered`와의 차이는 순차적으로 `index`가 정렬되었지 않다는 점이다.

### Non-Clustered 인덱스의 구조

![Nonclustered Index Structure](https://user-images.githubusercontent.com/49560745/107187245-0bda6e80-6a29-11eb-8053-9f7e8de7a9c9.png)

**Non-Clustered** 인덱스의 구조이다. **Clustured** 구조와는 다르게 **Leaf Level**과 **Data Page**가 구분된다. 그리고 **Data Page**의 데이터는 정렬 되어있지 않다.

<br/>

### 어떤 경우에 생성해야할까?

- `where`절이나 `Join` 절과 같이 조건문을 활용하여 테이블을 필터링 하고자할 때
- 데이터가 자주 업데이트 될 때
- 특정 컬럼이 쿼리에서 자주사용 될 때

<br/>

## 결론

|      | Clustered                                   | Non-Clustered                             |
| :--: | :------------------------------------------ | :---------------------------------------- |
|      | 항상 순서를 유지한다                        | 순서와 상관 없다                          |
|      | 한 테이블당 하나만 존재한다 (테이블 인덱스) | 한 테이블에 여러개 생성할 수 있다         |
|      | 범위 검색에 유리하다 (군집화!)              | index를 저장할 추가적인 공간이 필요하다   |
|      | 데이터가 많아 질수록 Insert 성능이 나빠진다 | Insert시 추가 작업 (인덱스 생성) 필요하다 |



- **Clustered** 인덱스는 테이블당 오직 한개만 존재한다. 반면에 **Non-Clustered** 형은 테이블 당 여러개의 인덱스를 생성할 수 있다.
- **Clustered** 인덱스는 오직 테이블을 정렬한다. 그러므로 별도의 공간을 필요로하지 않는다. **Non-Clustered** 인덱스는  저장되는 별도의 공간(약 10%)이 필요하다.
- **Clustered** 인덱스는 통상적으로 데이터를 찾는데 추가적인 스텝을 거치지 않기 때문에 **Non-Clustered** 인덱스보다 속도가 빠르다. 
- **Clustered** 인덱스는 데이터를 삽입할 때,  모든 테이블에 존재하는 데이터들의 순서를 유지해야하므로 많은 비용이 발생한다. **Non-Clustered**는 별도의 공간에 인덱스를 생성해야하기 때문에 추가작업이 필요하다.

<br/>

# Reference

-  [SQLShack - INDEX](https://www.sqlshack.com/what-is-the-difference-between-clustered-and-non-clustered-indexes-in-sql-server/)
-  [10분 테코톡 - INDEX](https://www.youtube.com/watch?v=NkZ6r6z2pBg)

- [인덱스(INDEX)의 모든 것](http://blog.naver.com/PostView.nhn?blogId=webwizard83&logNo=60171496664)

- [꽁담 - INDEX](https://mozi.tistory.com/320)
