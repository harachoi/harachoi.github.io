---
title: Java - 자바 코딩테스트 문법 정리
toc: true
categories:	
    - Java
tags: 
- Java
- 문법
- 자바 코딩테스트
last_modified_at:
---

   코딩테스트에서 자주 사용되는 `Java` 문법 정리, `Daily Update`

## ArrayList

### 깊은 복사

- `ArrayList`를 깊은 복사하고 싶다면, `복사되는배열.addAll(복사할배열)` 메서드를 사용하면된다.

- `ArrayList` **w**를 `ArrayList` **copy_w**에 깊은복사

```java
ArrayList<Integer> w=new ArrayList<Integer>();
ArrayList<Integer> copy_w=new ArrayList<Integer>();
copy_w.addAll(w);
```

### Sort

- `ArrayList` 를 정렬하고 할 때, `리스트명.sort()` 메소드를 사용한다.

```java
ArrayList<Integer> ArrList=new ArrayList<Integer>();
ArrList.sort(null);
```

### Size

- `ArrayList` 의 크기는 `리스트명.size()` 메소드를 사용한다.

```java
ArrayList<Integer> ArrList=new ArrayList<Integer>();
ArrList.size();
```



<br/>

## List

### set => List 변경

- 생성자에 값을 넣어주면,  `set -> List`로 변경할 수 있다.

```java
Set<String> set = new HashSet<String>();
List<String> menuList = new ArrayList<>(set);
```



### Sort

- `List` 를 정렬하고자 할 때, `Collections.sort(리스트명)` 메소드를 사용한다.

```java
List<Stirng> list = new ArrayList<>();
Collections.sort(menuList);
```



### Add

- `List` 에 값을 넣을 때, `리스트명.add(넣을 값)` 메소드를 사용한다.

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(1);
```

### remove

- `List` 의 값을 삭제할 떄, `리스트명.remove()` 메소드를 사용한다.
- `리스트명.remove(삭제할 값의 index)`

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.remove(list.size()-1); // list의 마지막 값이 리스트에서 제거된다.
```

### Size

- `List` 의 크기는 `리스트명.size()` 메소드를 사용한다.

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.size();
```



<br/>

## Array

### Sort

- `Array` 를 정렬하고자 할 때, `Arrays.sort(배열명)` 메소드를 사용한다.

```java
int [] Arr=new int[5];
Arrays.sort(Arr);
```

### length

- `Array` 의 길이는 `배열명.length` 를 사용한다.

```java
int [] Arr=new int[5];
Arr.length;
```

### String to Char Array

- `String`을 `char` 배열로 변환할 때, `toCharArray()` 메소드를 사용한다.

```java
char[][] board=new char[5][5];

// String 입력을 char형 Array로 변환
for(int i=0;i<5;i++){
	board[i]=br.readLine().toCharArray();
}

// String to char Array
String str="12345";
board[0]=str.toCharArray();
        
System.out.println(board[0]);

[출력]
12345
```

### 배열 특정 범위 자르기

- 배열에서 특정 범위를 자르고, 다른 배열에 저장할 때 `Arrays.copyOfRange(배열명,시작점,끝점)` 메소드를 사용한다.
- 이때, 범위는 `[시작점,끝점)` 형식으로 시작점 이상 끝점 미만의 범위가 설정된다.

```java
int[] array={1,2,3,4,5};
int[] temp=Arrays.copyOfRange(array,1,3);
System.out.println(temp);

[출력]
[2,3]
```



<br/>

## Set

### 값 넣기(add)

- `Set` 에 값을 넣을 때는, `set명.add(넣을 값)` 메소드를 사용한다.

```java
Set<String> set = new HashSet<String>();
set.add("combMenu");
```

### 값 삭제(remove)

- `Set` 에서 값을 삭제할 때는, `set명.remove(삭제할 값)` 메소드를 사용한다.

```java
Set<String> set = new HashSet<String>();
set.remove("combMenu");
```



### Iterator

- `set` 의 값을 조회할 떄, `set명.iterator()`를 사용해 반복자를 생성한다.
- `반복자.hasNext()` 메소드로 다음 값이 존재하는지 확인한다.
- `반복자.next()` 메소드로 참조값을 가져온다.

```java
Set<String> set= new HashSet<String>();

set.add("1");
set.add("2");
set.add("3");

Iterator<String> it= set.iterator();
while(it.hasNext()){
	String a= it.next();
	System.out.println(a);
}

[결과]
1
2
3
```

### size

- `set` 의 크기는 `set명.size()` 메소드를 사용한다.

```java
Set<String> set= new HashSet<String>();
set.size();
```





<br/>

## Map

### 값 넣기(put)

- `Map`에 `{key : value}` 값을 설정할 때, `map명.put(key,value)` 메소드를 사용한다.

```java
Map<String,Integer> map=new HashMap<>();
map.put("str",1);
```

### 값 가져오기(get)

- `Map`의 `{key:value}`쌍의 value` 값을 가져올 때`,  `map명.get(key값)` 메소드를 사용한다.

```java
Map<String,Integer> map=new HashMap<>();
map.get("str");
```



### Key 값 존재 확인(containsKey)

- `Map` 에 해당하는 `key` 값이 존재하는지 확인할 때, `map명.containsKey(key값)` 메소드를 사용한다.
- `Key` 값이 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.

```java
Map<String,Integer> map=new HashMap<>();
map.containsKey("str");
```

### Iterator

- `map` 의 값을 조회할 떄, `map명.keySet().iterator()`를 사용해 반복자를 생성한다.
- `반복자.hasNext()` 메소드로 다음 값이 존재하는지 확인한다.
- `반복자.next()` 메소드로 참조값을 가져온다.

```java
Map<String,Integer> map=new HashMap<>();
Iterator<String> it= map.keySet().iterator();

while(it.hasNext()){
	String key=it.next();
	int value=map.get(key);
}
```



### size

- `Map`의 크기는 `map명.size()` 를 사용한다.

```java
Map<String,Integer> map=new HashMap<>();
map.size();
```

### Map에 value 값으로 java Collection 넣어주기

- `Map`에 `value` 값으로 `collection`을 넣는 방법이다.
- 2차원 배열을 사용하고는 싶은데, 배열의 `index`를 `Int`형이 아닌 `String` 혹은 `Char` 형으로 지정하고 싶을 때 사용하면 된다.
- ex) map["abc"]={1,2,3,4,5}

```java
Map<String,List<Integer>> map=new HashMap<>();

// map이 비어있다면 list를 만들어 넣어준다.
if(map.containsKey(str)==false){
	List<Integer> list=new ArrayList<>();
	list.add(Integer.parseInt(info[4]));
	map.put(str,list);
}else{
    
// map이 비어있지 않다면 list에 값을 넣어준다
	map.get(str).add(Integer.parseInt(info[4]));
}
```



<br/>

## String

### 소문자, 대문자

- `String`문자열의 문자 값을 `대 -> 소`로 변경할 때, `toLowerCase()` 메소드를 사용한다.

````java
String str="ABC";
str=str.toLowerCase();
//"abc";
````

- `String`문자열의 문자 값을 `소 -> 대`로 변경할 때, `toUpperCase()` 메소드를 사용한다.

```java
String str="abc";
str=str.toLowerCase();
//"ABC";
```

### String to Array

- `String` 문자열을 `Array`로 만들 때, `스트링명.split()` 메소드를 사용한다.

```java
String str = "12345";
String[] Arr = str.split("");
//[1,2,3,4,5]
```

### 문자열 자르기(substring)

- `String` 문자열 일부를 추출할 때, `스트링명.substring()` 메소드를 사용한다.
- `스트링명.substring(idx)` : `idx`를 포함한 위치부터 문자열 끝까지 추출한다.
- `스트링명.substring(시작값,끝 값)` : `시작값` 부터 `끝 값 -1` 까지의 문자열을 추출한다.

```java
String str="1234567";
str.substring(3); // "4567"
str.substring(2,5) // "345"
```

### 문자열 뒤집기(Reverse)

- `String` 문자열을 뒤집기 할 때, `StringBuilder(문자열).reverse().toString()` 메소드를 사용한다.

```java
String str = "Reverse";
String str = new StringBuilder(words).reverse().toString();
System.out.println(str); 

[출력]
esreveR
```

### length

- `String` 의 길이는 `문자열명.length()` 를 사용한다.

```java
String str="123";
str.length();
```

### String 값 변경하기

- `Java`에서 `String`은 `immutable`하다. 즉, 한번 할당되면 변경이 불가능하다.
- 따라서, 특정 문자열을 변경하려면 `substring` 메소드를 활용해 변경된 새로운 문자열을 생성해야한다.

```java
String name="starfucks";
String newname=name.substring(0,4)+'b'+name.substring(5);

System.out.pirntln(newname); // starbucks
```



<br/>

## StringBuilder

- `StringBuilder` 클래스로 문자열에 문자를 추가하거나, 삭제할 수 있다.

### 추가(append)

- **추가**할 때는, `빌더명.append(넣을문자)` 메소드를 사용한다.

```java
StringBuilder sb=new StringBuilder();

sb.append('a');
sb.append('b');
sb.append('c');

System.out.println(sb);

[출력]
abc
```



### 삭제(deleteChartAt)

- **삭제**할 때는, `빌더명.deletCharAt(삭제할문자의 인덱스)` 메소드를 사용한다.

```java
StringBuilder sb=new StringBuilder();

sb.append('a');
sb.append('b');
sb.append('c');

System.out.println(sb);

sb.deleteCharAt(1);

System.out.println(sb);

[출력]
abc
ac
```

### StringBuilder 값 변경하기

- `StringBuilder` 객체의 특정 값을 변경할 때, `빌더명.setChartAt(인덱스,문자)` 메소드를 활용한다.

```java
StringBuilder name = new StringBuilder("starfucks");
name.setCharAt(4, 'b');

System.out.println(name); // starbucks
```



<br/>

## 입/출력

### BufferdReader

- `BurredReader` 클래스는 `Enter`를 경계로 입력값을 인식한다.
- `readLine()` 메소드는  개행문자(`Enter`)를 포함해 `String` 형식으로 입력을 받아온다.
  - 따라서, `int`형으로 `readLine()` 을 받아오려면 `Integer.pareseInt()` 로 형변환이 필요하다.

```
[입력]
100
```

```java
import java.io.InputStreamReader;
import java.io.BufferedReader;
import java.io.IOException;
public class InputOuputTest {
    public static void main(String args[])throws IOException{
        BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
        int N=Integer.parseInt(br.readLine()); 
        System.out.prin(N);
    }
}

[출력]
100
```



### StringTokenizer

- `StringTokenizer` 클래스는 지정한 형식에 따라 문자열을 쪼개주는 클래스이다.
- `new StringTokenizer(문자열,기준)` 형식으로 `StringTokenizer` 객체를 생성할 수 있다.

```
[입력]
123 456
```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class InputOuputTest {
    public static void main(String args[])throws IOException{
        BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st= new StringTokenizer(br.readLine());

        int N= Integer.parseInt(st.nextToken());
        int M= Integer.parseInt(st.nextToken());

        System.out.print(N+" "+M);
    }
}

[출력]
123 456
```



### 예시 

#### 1)

```
[입력]
3 3
111
222
333
```

```java
import java.util.*;
import java.io.*;

public class InputOuputTest {
    public static void main(String args[])throws IOException{
        BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st= new StringTokenizer(br.readLine());

        int N= Integer.parseInt(st.nextToken());
        int M= Integer.parseInt(st.nextToken());

        int [][]arr= new int[N][M];
        for(int i=0;i<N;i++){
            String str=br.readLine();
            for(int j=0;j<M;j++){
                arr[i][j]=Integer.parseInt(String.valueOf(str.charAt(j)));
            }
        }
        System.out.print(N+" "+M);
        for(int i=0;i<N;i++){
            for(int j=0;j<M;j++){
                System.out.print(arr[i][j]);
            }
            System.out.println();
        }
    }
}

    
[출력]
3 3
111
222
333
```

#### 2)

```
[입력]
5
1
2
3
4
5
```

```java
import java.util.*;
import java.io.*;

public class InputOuputTest {
    public static void main(String args[])throws IOException{
        BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
        int N=Integer.parseInt(br.readLine());
        for(int i=0;i<N;i++){
			int M=Integer.parseInt(br.readLine());
            System.out.println(M);
        }
    }
}

[출력]
1
2
3
4
5
```

## Queue

### 생성

- `JAVA`에서 자료구조 `Queue` 를 생성하는 방법은 아래와같다.

```java
import java.util.Queue;
Queue<Integer> queue=new LinkedList<>();
Queue<String> queue= new LinkedList<>();
```

### 삽입(add)

- `Queue`에 원소를 추가할 때는, `큐명.add(원소)` 메소드를 사용한다.

```java
Queue<Integer> queue=new LinkedList<>();
queue.add(1);
queue.add(2);
```

### 꺼내기(poll)

- `Queue`의 가장 앞의 원소를 꺼낼때는, `큐명.poll()` 메소드를 사용한다.

```java
Queue<Integer> queue=new LinkedList<>();
queue.add(1);
int que=queue.poll();
```

### pair

- `JAVA`에서는 `Pair` 연산자는 간단해서 보통 구현하여 사용한다.
- 아래 코드에서 입맞에 맞게  `Node` 클래스를 변경하고, 사용하면된다.

```java
static class Node{
        int y;
        int x;
        int dist;
        Node(int y,int x,int dist){
            this.y=y;
            this.x=x;
            this.dist=dist;
       }
   }

Queue<Node> queue=new LinkedList<>();
queue.add(new Node(1,2,3));
Node node= queue.poll();
```

## PriorityQueue

### 생성

- `JAVA`에서 자료구조 `PriorityQueue` 를 생성하는 방법은 아래와같다.
- 생성자에 `Collections` 를 사용해 `오름차순`, `내림차순`을 설정할 수 있다.(`default` : `오름차순`)

```java
import java.util.PriorityQueue;
/*
	오름차순
*/
PriorityQueue<Integer> pq=PriorityQueue<Integer>();
PriorityQueue<String> pq=PriorityQueue<String>();

/*
	내림차순
*/
PriorityQeueu<Integer> pq=PriorityQueue<Integer>(Collections.reverseOrder());
```

### 삽입

- `PriorityQueue` 에 원소를 삽입할 때, `pq명.add(원소)` 메소드를 사용한다.

```java
PriorityQueue<Integer> pq=PriorityQueue<Integer>();
pq.add(1); 
pq.add(2);
```

### 제거(remove)

- `PriorityQueue`의 가장 앞의 원소를 제거할 때, `pq명.remove()` 메소드를 사용한다.

```java
PriorityQueue<Integer> pq=new PriorityQueue<>(Collections.reverseOrder());
pq.add(2);pq.add(12);pq.add(13);

System.out.println(pq);
pq.remove();
System.out.println(pq);

[출력]
[13, 2, 12]
[12, 2]
```



### minHeap & maxHeap

- `PriorityQueue`의 `minHeap`, `maxHeap` 에 접근할 때, `pq명.peek()` 메소드를 사용한다.

```java
PriorityQueue<Integer> pq=PriorityQueue<Integer>();
pq.add(1); 
pq.add(2);
pq.add(13); 

System.out.println(pq.peek()); // 1
```

```java
PriorityQueue<Integer> pq=PriorityQueue<Integer>(Collections.reverseOrder());
pq.add(1); 
pq.add(2);
pq.add(13); 

System.out.println(pq.peek()); // 13
```

### pair

- `PriorityQueue` 에서 `pair`를 사용방법은 아래와 같다.
- `compare` 메소드를 만들고, 생성자에 포함시키면 된다.

```java
import java.io.IOException;
import java.util.PriorityQueue;
public class PQ {

    static class Node{
        int y;
        int x;

        Node(int y,int x){
            this.y=y;
            this.x=x;
        }

        public int compareTo(Node p) {
            if(this.y < p.x) {
                return -1; // 오름차순
            }
            else if(this.y == p.y) {
                if(this.x < p.x) {
                    return -1;
                }
            }
            return 1;
        }
    }

    public static void main(String[] args) throws IOException{

        PriorityQueue<Node> pq1=new PriorityQueue<>(Node::compareTo);
        pq1.add(new Node(1,2));
        pq1.add(new Node(1,1));
        pq1.add(new Node(2,3));
        pq1.add(new Node(2,1));

        while(!pq1.isEmpty()){
            Node node=pq1.peek();
            System.out.println(node.y+" "+node.x);
            pq1.remove();
        }
    }
}

[출력]
1 1
1 2
2 1
2 3
```

## 인접리스트 구현

 `Java`에서 인접리스트를 구현하는 방법이다.

![인접리스트](https://user-images.githubusercontent.com/49560745/107134469-b5403800-6935-11eb-90e4-d01e40b8c287.png)

```java
// 단 방향 [출발노드,도착노드] 가 주어졌을 때
int[][] arr=[[1,3],[1,5],[3,2],[3,4],[5,4],[5,6],[2,4],[4,6]];

ArrayList<ArrayList<Integer>> list=new ArrayList<>();

// 인접리스트 초기화
for(int i=0;i<=arr.length;i++){
	list.add(new ArrayList<>());
}

// 양방향 인접리스트
for(int i=0;i<arr.length;i++){
    int start=arr[i][0],end=arr[i][1];
	list.get(start).add(end);
    list.get(end).add(start);
}
```

