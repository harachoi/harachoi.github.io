---
title: InsetionSort(삽입정렬) - JAVA
toc: true
categories:	
    - Algorithm non PS
tags:
- 알고리즘
- 삽입정렬
last_modified_at: 
---

 **InsetionSort**의 개념을 간단하게 알아보고, `java`로 구현해보자.

## 삽입 정렬이란?

- 자료 배열의 모든 요소를 앞에서부터 차례대로 **이미 정렬된 배열 부분**과 비교하여, 자신의 위치를 찾아 **삽입**함으로써 정렬을 완성하는 알고리즘

## 삽입 정렬 과정

```
1) 2번째 index부터 key값을 잡는다.
2) key값에서 역순으로 탐색한다.
3) key 자신보다 작은 값을 찾거나 첫번째 index에 도달하면
그 값과 swap 한다.
```

`[3,7,2,5,1,4]` 리스트를 **InsetionSort**로 정렬해보자.

- `2번째` index부터 `key`값을 설정하고, 역순으로 정렬 된 배열을 탐색하며 `key` 자신보다 큰 값을 뒤로 밀어준다. 
- `key` 자신보다 작거나 첫 index에 도달했을 때 `key` 값을 그 위치에 삽입한다.
- 항상 `1번째` index는 정렬이되어있다고 가정한다.

![InsertionSort](https://user-images.githubusercontent.com/49560745/108151465-14623180-711a-11eb-904b-c3b2613895cb.png)

![InsertionSort](https://user-images.githubusercontent.com/49560745/108151518-3cea2b80-711a-11eb-813b-58608460541e.png)

![InsertionSort](https://user-images.githubusercontent.com/49560745/108152127-b33b5d80-711b-11eb-8b66-e326971ae4b7.png)

![InsertionSort](https://user-images.githubusercontent.com/49560745/108151559-57bca000-711a-11eb-8739-4ec9d38045ea.png)



![InsertionSort](https://user-images.githubusercontent.com/49560745/108151592-6c009d00-711a-11eb-96fe-66f50aab966e.png)

![InsertionSort](https://user-images.githubusercontent.com/49560745/108151772-d3b6e800-711a-11eb-91a3-3473c748d6a3.png)

## 시간복잡도(Time Complexity)

- best : `O(N)`

이동없이 1번의 루프만을 돈다.

- worst : `O(N^2)`

각 루프마다 매번 이동이 발생할 때, 시간복잡도는 항상 `O(N^2)`이다.

<br/>

## 구현

```java
public class insertionSort {
    static void insertionSort(int[] list){
        int i=0,j=0,key=0;
        for(i=1;i<list.length;i++){
            key=list[i];
            for(j=i-1;j>=0 && list[j]>key;j--){
                list[j+1]=list[j];
            }
            list[j+1]=key;
        }
    }

    public static void main(String[] args){
        int[] list={3,7,2,5,1,4};
        insertionSort(list);
        for(int i=0;i<list.length;i++){
            System.out.print(list[i]+",");
        }
    }
}

```

<br/>

# Reference

- [삽입정렬 - 위키백과 ](https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC)