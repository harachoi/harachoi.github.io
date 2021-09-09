---
title: SQL 쿼리 실행 과정 (6 STEP)
toc: true
categories:	
    - Database
tags:
- 데이터베이스
- sql
- 쿼리 실행 과정
last_modified_at: 
---

 

 이번 포스팅에서는 `SQL` 쿼리의 실행과정에 대해 알아보려 한다. `SQL` 쿼리의 실행과정을 이해한다면, 쿼리문을 조금 더 수월하게 작성할 수 있다.



##  쿼리 프로세스

`SQL`의 쿼리문 실행 프로세스는 크게 **6 단계**로 구분할 수 있다.

```
1. 데이터 가져오기 (From, Join)
2. 조건문 행 필터링 (Where)
3. 그룹핑 (Group by)
4. 그룹핑 결과 필터링 (Having)
5. 결과 리턴 (Select)
6. 결과 정렬 (Order by & Limit / Offset)
```

기본적인 흐름을 눈대중으로 파악했다면, 예제를 통해 실제로 어떻게 동작되는지 확인해보자. 예제 테이블은 아래와 같다.

<br/>

![쿼리프로세스-table](https://user-images.githubusercontent.com/49560745/107364626-08291380-6b1f-11eb-97c3-8a1549ac47f3.png)



**Empyolee** 테이블에는 직원의 **성**과 직무의 **ID 번호**가 하나의 행으로 데이터가 저장되어있다. 그리고 **JOB** 테이블에는 직무의 **ID 번호**와 이에 해당하는 **직무 이름**으로 데이터가 구성되어있다. 

이제, 두 테이블에서 

1) 직무가 **Front-End** 이거나 **Back-End** 이면서,

2) 같은 직무 동료가 두 명 이상인 데이터를 

3) **직무 이름** 기준으로 오름차순 정렬하는 쿼리문을 실행해보자.

```javascript
SELECT job.jobName
FROM Emplyoee
JOIN job
ON Emplyoee.jobid= job.jobid 
WHERE job.jobName != 'AI'
GROUP BY job.jobName 
HAVING COUNT(*) >= 2
ORDER BY job.jobName ASC
```

 

## 1. 데이터 가져오기 (From, Join)

```
FROM Emplyoee
JOIN job
```

쿼리문이 실행되면 우선 베이스 데이터를 가져온다. 우선, `From` 절을 실행하고, `Join` 절을 실행한다. 위 쿼리의 연산결과는 [데카르트 곱](https://ko.wikipedia.org/wiki/%EA%B3%B1%EC%A7%91%ED%95%A9) 형태로 나타나게 된다.

![image](https://user-images.githubusercontent.com/49560745/107368558-09a90a80-6b24-11eb-9ce3-d5674462eb94.png)

이후에 **ON** 조건문이 실행되어 [데카르트 곱](https://ko.wikipedia.org/wiki/%EA%B3%B1%EC%A7%91%ED%95%A9) 결과로부터 `Emplyoee.jobid= job.jobid ` 조건문이 필터링 된 결과값이 나오게된다.

![image](https://user-images.githubusercontent.com/49560745/107368849-63a9d000-6b24-11eb-8962-a872adf5f0da.png)

## 2. 조건문 필터링 (Where)

```
WHERE job.jobName != 'AI'
```

그 다음으로 `WHERE` 조건문이 실행된다. 해당 조건에 알맞지 않은 데이터는 필터링으로 걸러진다.

![image](https://user-images.githubusercontent.com/49560745/107368890-70c6bf00-6b24-11eb-8210-9d499c796c70.png)

## 3. 그룹핑 (Group by)

```
GROUP BY job.jobName 
```

조건문으로 필터링 과정이 이루어졌으면, 이젠 그룹핑 작업이 진행된다.  

![image](https://user-images.githubusercontent.com/49560745/107369170-c69b6700-6b24-11eb-9afd-6f7d51d88fee.png)

## 4. 그룹핑 필터링 (Having)

```
HAVING COUNT(*) >= 2
```

`GROUP BY` 결과 이후 `Having` 절이 실행된다. `Having` 절은 이제 개별적인 행에 접근할 수 없으며, `Group` 단위로만 조건이 적용된다.

<br/>

## 5. 결과 return (Select)

```
SELECT job.jobName
```

이제 `ON`, `WHERE` 조건문으로 필터링되고, `GROUP BY` 절로 그룹핑 된 후 `HAVING` 절에 의해 그룹핑된 결과도 필터링된 결과값이 도출된다.

![image](https://user-images.githubusercontent.com/49560745/107371263-6d810280-6b27-11eb-8b3f-b028a4e96850.png)



## 6. 결과 정렬 (Order by & Limit)

```
ORDER BY job.jobName ASC
```

모든 `SQL` 쿼리문이 실행된 이후에 가장 마지막으로 데이터의 최종 정렬이 이루어진다. 이 과정에서 `ORDER BY` , `LIMIT` 절이 실행된다. 최종 결과값은 다음과 같다.

![image](https://user-images.githubusercontent.com/49560745/107371714-f26c1c00-6b27-11eb-9980-5dc2825f2a90.png)

<br/>

# Reference

-  [towards - The 6 Steps of a SQL Select Statement Process](https://towardsdatascience.com/the-6-steps-of-a-sql-select-statement-process-b3696a49a642)
