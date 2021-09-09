---
title: SelectionSort(선택정렬) - JAVA
toc: true
categories:	
    - Algorithm non PS
tags:
- 알고리즘
- 선택정렬
last_modified_at: 
---

 **SelectionSort**의 개념을 간단하게 알아보고, `java`로 구현해보자.

## 선택 정렬이란?

- 제자리 정렬 알고리즘
- 가장 단순한 정렬 알고리즘이며, 메모리가 제한적인 경우 성능 이점

## 선택 정렬 과정

```
1) 주어진 리스트 중에 최소값을 찾는다.
2) 그 값을 맨 앞에 위치한 값과 교체한다(패스(pass)).
3) 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.
```

## 시뮬레이션

`[7,4,2,8,3,5,1,6,10,9]` 리스트를 **SelectionSort**로 정렬해보자.

- 최소값을 찾는다.

![selectionSort](https://user-images.githubusercontent.com/49560745/108148688-b1ba6700-7114-11eb-9d79-61202a08666c.png)

- 현재 인덱스에 위치한 `7`과 최소값 `1`을 `SWAP`하고, 인덱스를 하나 증가시킨 후 `MinValue`를 찾는다. 이 과정을 정렬이 완료될 때까지 반복한다.

![selectionSort](https://user-images.githubusercontent.com/49560745/108148779-da426100-7114-11eb-9dcd-07c98881dee3.png)



![selectionSort](https://user-images.githubusercontent.com/49560745/108148852-03fb8800-7115-11eb-94c5-84e0a31b8524.png)

![selectionSort](https://user-images.githubusercontent.com/49560745/108148884-137ad100-7115-11eb-965a-68bd287adb86.png)

![selectionSort](https://user-images.githubusercontent.com/49560745/108148919-27263780-7115-11eb-98ac-3d6b10857209.png)

![selectionSort](https://user-images.githubusercontent.com/49560745/108148937-32796300-7115-11eb-99b3-92f9a688f144.png)



![selectionSort](https://user-images.githubusercontent.com/49560745/108148988-458c3300-7115-11eb-889b-45abf26754cc.png)

- `MinValue`가 더 크기 때문에 `SWAP`하지 않는다.

![selectionSort](https://user-images.githubusercontent.com/49560745/108149015-53da4f00-7115-11eb-8e79-02c1edf8e846.png)

![selectionSort](https://user-images.githubusercontent.com/49560745/108149103-753b3b00-7115-11eb-9385-e38d67693f79.png)

![selectionSort](https://user-images.githubusercontent.com/49560745/108149137-84ba8400-7115-11eb-9f34-afc6592afcf4.png)





## 시간복잡도(Time Complexity)

- `O(N^2)`

탐색 횟수가 항상 고정적이므로 시간복잡도는 항상 `O(N^2)`이다.

<br/>

## 구현

```java
public class selectionSort{
    static void selectionSort(int[] list) {
    int indexMin, temp;

    for (int i = 0; i < list.length - 1; i++) {
        indexMin = i;
        
         // 최솟값을 탐색한다.
        for (int j = i + 1; j < list.length; j++) {
            if (list[j] < list[indexMin]) {
                indexMin = j;
            }
        }
        // 최솟값이 자기 자신이면 자료 이동을 하지 않는다.
        if(i!=indexMin){
            temp = list[indexMin];
            list[indexMin] = list[i];
            list[i] = temp;
        }
    }
}
    
    public static void main(String[] args){
     	int[] list={7,4,2,8,3,5,1,6,10,9};
        selectionSort(list);
        for(int i=0;i<list.length;i++) {
            System.out.print(list[i]+",");
        }
    }
}

```

<br/>

# Reference

- [선택정렬 - 위키백과 ](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)