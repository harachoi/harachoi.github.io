---
title: BubbleSort(버블정렬) - JAVA
toc: true
categories:	
    - Algorithm non PS
tags:
- 알고리즘
- 버블정렬
last_modified_at: 
---

 **BubblSort**의 개념을 간단하게 알아보고, `java`로 구현해보자.

## 거품 정렬이란?

- 두 인접한 원소를 검사하며 정렬하는 알고리즘

- 가장 단순한 정렬 알고리즘이지만, 시간복잡도가 `O(N^2)`으로 상당히느리다.

## 거품 정렬 과정

```
인접한 두 원소를 비교하면서 큰 값이 작은값보다 뒤에 위치하도록 정렬한다.
```

## 시뮬레이션

`[3,4,1,6,7,5]` 리스트를 **BubbleSort**로 정렬해보자.

- 뒤에서부터 정렬을 완성시킨다.
- 매번 첫원소부터 비교하면서, 인접한 원소보다 크다면 뒤에 위치하도록 `SWAP`해준다.



- `3<4` 이므로 건너뛴다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108160541-9d826400-712c-11eb-960b-3c0b2c616f6b.png)

- `4>1` 이므로 `SWAP`한다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108160615-bd198c80-712c-11eb-8f60-6dfa4c5c34fd.png)

- `4>6` 이므로 건너뛴다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108160653-d1f62000-712c-11eb-9572-7801b01e5177.png)

- `6>7`이므로 건너뛴다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108160687-e508f000-712c-11eb-9ab2-c1d8b2ad2bee.png)

- `5<7`이므로 `SWAP`한다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108160725-f94ced00-712c-11eb-98df-318b49f56236.png)

이제, 한 `STEP`이 끝났다. 위의 과정을 `list`의 길이만큼 반복하면 오름차순 정렬이 완료된다.

- 이어서 정렬 해보자.
- `1>3`이므로 `SWAP`해준다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108161130-c1927500-712d-11eb-8502-c3f10bfc4cdf.png)

- `3<4` 이므로 건너뛴다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108161167-d2db8180-712d-11eb-84f6-eacb5ed485c3.png)

- `4<6` 이므로 건너뛴다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108161217-ec7cc900-712d-11eb-8df2-beb04240426a.png)

- `6>5`이므로 `SWAP` 한다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108161335-2c43b080-712e-11eb-9c9b-e6eefb585275.png)

- 정렬이 완료되었다.

![BubbleSort](https://user-images.githubusercontent.com/49560745/108161359-36fe4580-712e-11eb-8928-4d80c83dc214.png)

## 시간복잡도(Time Complexity)

- `O(N^2)`



<br/>

## 구현

```java
public class bubbleSort {

    static void bubbleSort(int[] list){
        for(int i=0;i<list.length-1;i++){
            for(int j=1;j<list.length-i;j++){
                if(list[j]<list[j-1]){
                    int tmp=list[j];
                    list[j]=list[j-1];
                    list[j-1]=tmp;
                }
            }
        }
    }

    public static void main(String[] args){
        int[] list={3,4,1,6,7,5};
        bubbleSort(list);
    }
}

```

<br/>

# Reference

- [거품정렬 - 위키백과](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)

