---
title: Datastructure 기술 면접 정리
toc: true
categories:	
    - Interview
tags:
- Spring
last_modified_at: 
---



`Daily Update`

## Datastructure

### Map vs HashTable

#### Map

- `Map`은 레드블랙트리 기반의 자료구조로 모든 자료가 정렬되어 저장된다.
- `Olog(n)`을 보장하며 자료를 정렬한다.
- 키 값의 분포가 고르지 않을 때 `insert`, `delete` 성능이 저하될 수 있다.

#### HashTable

- `Table`을 통해 데이터를 참조하므로 정렬이 필요없다.
- 그렇기 때문에 `search`, `insert`, `delete` 의 성능이 `O(1)`으로 보장된다.

### Set

#### Set 특징

- 데이터를 비순차적으로 저장할 수 있는 순열 자료구조(Collection)
- 데이터는 삽입 순서대로 저장되지 않아 특정 인덱스에 접근할 수 없다.
- **중복을 허용하지 않아** 중복된 값의 경우 가장 마지막의 값만 저장된다.
- 주로 특정 데이터가 존재하는지 알고 싶을 때 사용한다.

#### Set 구조

- 저장할 데이터의 **Hash 값**을 구한다.
- **Hash 값**에 해당하는 공간(Bucket)에 값을 저장한다.