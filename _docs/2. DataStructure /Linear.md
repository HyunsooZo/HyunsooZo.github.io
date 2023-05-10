---
title: Linear Data Structure
category: DataStructures
order: 1
---
### Linear Data Structure? 


선형 자료구조(Linear Data Structure)란, 자료들이 일렬로 늘어선 형태로 구성되어 있는 자료구조를 말한다. 이
러한 자료구조는 각 원소들 간에 순서가 존재하며, 특정 원소에 접근하기 위해서는 그 원소 이전의 모든 원소들을 순차적으로 접근해야 한다.

대표적인 선형 자료구조로는 배열(Array), 연결 리스트(Linked List), 스택(Stack), 큐(Queue), 데크(Deque), 해시 테이블(Hash Table)이 있다. 

각각의 자료구조는 자료의 삽입, 삭제, 검색 등의 연산에 따라서 효율성이 다르므로 프로그래밍 시 적절한 자료구조를 사용하는 것이 중요하다.


### Time complexity


| 자료구조 | 탐색 | 삽입 | 삭제 |
| -------- | ---- | ---- | ---- |
| 배열     | O(1) | O(n) | O(n) |
| 링크드 리스트 | O(n) | O(1) | O(1) |
| 스택     | O(n) | O(1) | O(1) |
| 해시 테이블  | O(1) | O(1) | O(1) |
| 큐      | O(n) | O(1) | O(1) |
| 데크     | O(n) | O(1) | O(1) |


### Array

<div class="content-box">
많은 수의 데이터를 다룰때 사용하는 자료구조. 
각 데이터를 인덱스와 1:1 대응하도록 구성한다.
데이터가 메모리상에 연속적으로 저장됨. 
</div>

|Pros|Cons|
|--|--|
|Index통해 데이터에 빠르게 접근가능|데이터의 추가/삭제가 번거로움|
||최대길이를 미리 정하여 생성해야함|
||가변길이 배열은 배열의 크기변경 시마다 새로운 배열 생성|
||삭제 시 Index유지를 위해 빈 공간 유지|


**[Sample]**
```java
//array creation 
int[] arr = new int[n]                  // 배열 생성 (길이 n)
int[][] arrMatrix = new int[n][n]       // n x n 의 이차원 배열 생성 

//stream api methods
Arrays.strean(arr)
                  .boxed()              //object로 변환
                  .sorted()             // 정렬
                  .filter(x->x==1)      //값이 1인 요소들만 필터링
                  .anyMatch(x->x==1)    // 값이 1인 요소가 있는지 확인 
                  .count()              // 갯수 확인 (long 타입)
                  .min().getAsInt()     // 최솟값 확인 (int 타입)
                  .max().getAsInt()     // 최댓값 확인 (int 타입)
                  .map()                // 데이터타입 변경 (주로 -> object로..)
                  .maptoInt()           // 데이터타입 Int로 편입 (예 : Integer::parseInt 등)
                  .sum()                // 요소들의 합 
                  .average()            // 요소들의 평균
                  
// IntStream & methods
IntStream()       
                  .range(int startInclusive, int endExclusive)
                  // startInclusive부터 endExclusive-1까지 정수 값으로 이루어진 새로운 IntStream 생성
```


### Linked List

<div class="content-box">
data를 link하여 관리하는 구조
자료의 순서가 정해져있음. 
단 메모리의 연속성을 보장하는것은 아님. 
</div>

|Pros|Cons|
|--|--|
|데이터 공간을 미리 할당하지 않아도 됨|연결구조를 위한 별도 데이터 공간필요|
|리스트의 길이가 가변적이라 데이터 삭제/추가 용이|연결정보 탐색 시간 필요(비교적 느림)|
||데이터 추가/삭제 시 앞 뒤 데이터의 연결 재구성필요|

**[sample]**
~~~java
//LinkedList creation
LinkedList<String> list = new LinkedList<>();    

.add(E element)               //지정된 요소를 리스트의 끝에 추가
.add(int index, E element)    //지정된 위치(index)에 요소를 삽입
.addAll(Collection<? extends E> c)       
.addAll(int index, Collection<? extends E> c)
.clear()                     //리스트의 모든 요소를 제거
.contains(Object o)          //리스트가 지정된 요소(o)를 포함하고 있는지 확인
.get(int index)              //지정된 위치(index)에 있는 요소를 반환
.indexOf(Object o)           //리스트에서 지정된 요소(o)의 첫번째 등장 위치의 인덱스를 반환
.isEmpty()                   //리스트가 비어있는지 확인
.iterator()                  //리스트의 요소를 반복하는데 사용할 수 있는 Iterator 객체를 반환            
.remove(Object o)            //리스트에서 지정된 요소(o)를 제거
.remove(int index)           //지정된 위치(index)에 있는 요소를 제거
.size()                      //리스트의 크기를 반환
.toArray()                   //리스트의 모든 요소를 배열로 반환

// Methods could be useful for Queue, Deque
.addFirst(E element)                               //리스트의 맨 앞에 지정된 요소를 추가
.addLast(E element)                                //리스트의 맨 끝에 지정된 요소를 추가
.getFirst()                                        //리스트의 첫번째 요소를 반환
.getLast()                                         //리스트의 마지막 요소를 반환
.removeFirst()                                     //리스트의 첫번째 요소를 제거
.removeLast()                                      //리스트의 마지막 요소를 제거
~~~


### Stack

<div class="content-box">
LIFO(Last In First Out) 구조
데이터가 입력된 순서의 역순으로 처리해야할 내용에 유리(함수콜스택/수식/인터럽트 처리 등)
사실 Stack Queue 보다는 Deque 사용하는 것이 시간효율면에서 유리 
</div>

**[sample]**
```java
//Stack creation
Stack<String> stack = new Stack<>();  

.empty()            // 스택이 비어있는지 확인
.peek()             // 스택의 맨 위에 있는 요소를 반환. 스택이 비어있을 경우 null을 반환
.pop()              // 스택의 맨 위에 있는 요소를 제거하고 반환. 스택이 비어있을 경우 EmptyStackException을 발생
.push(E item)       // 지정된 요소를 스택의 맨 위에 추가
.search(Object o)   // 지정된 요소(o)가 스택의 위치 반환. 가장 최근에 추가된 요소=1,스택에 없을 경우 -1을 반환합니다.
```


### Queue 

**Queue**

<div class="content-box">
FIFO(First In First Out) 구조
입력 순서대로 데이터 처리가 필요한 경우 유리(프린터 출력대기열, BFS등..)
빈 큐에서 poll() 시 Stack은 Error / Queue 는 null 반환
</div>

**[sample]**
```java
// new LinkedList<>() in General 
Queue<String> queue = new LinkedList<>();


//Methods for Queue
.add(e)       // 지정된 요소(e)를 큐의 끝에 추가. 큐가 가득 차 있을 경우 IllegalStateException을 발생
.offer(e)     // 지정된 요소(e)를 큐의 끝에 추가. 큐가 가득 차 있을 경우 false를 반환
.poll()       // 큐의 맨 앞에 있는 요소를 제거하고 반환. 큐가 비어있을 경우 null을 반환
.element()    // 큐의 맨 앞에 있는 요소를 반환. 큐가 비어있을 경우 NoSuchElementException을 발생
.peek()       // 큐의 맨 앞에 있는 요소를 반환. 큐가 비어있을 경우 null을 반환.
```


### Deque

<div class="content-box">
양쪽에서 삽입/삭제 가능
Double Ended Queue임. 
Stack 과 Queue를 합친 형태라고 볼 수 있음. 
Front 또는 Reat의 입력을 제한하여 입력제한(scroll) 또는 출력제한(shelf)로 사용 할 수 있음.  
</div>

**[sample]**
```java
Deque<String> deque = new LinkedList<>(); 
//Methods for Deque
.addFirst(e)  // 지정된 요소(e)를 덱의 맨 앞에 추가. 덱이 가득 차 있을 경우 IllegalStateException을 발생
.addLast(e)   // 지정된 요소(e)를 덱의 맨 끝에 추가. 덱이 가득 차 있을 경우 IllegalStateException을 발생
.offerFirst(e)// 지정된 요소(e)를 덱의 맨 앞에 추가. 덱이 가득 차 있을 경우 false를 반환
.offerLast(e) // 지정된 요소(e)를 덱의 맨 끝에 추가. 덱이 가득 차 있을 경우 false를 반환
.removeFirst()// 덱의 맨 앞에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 NoSuchElementException을 발생
.removeLast() // 덱의 맨 끝에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 NoSuchElementException을 발생
.pollFirst()  // 덱의 맨 앞에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 null을 반환
.pollLast()   // 덱의 맨 끝에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 null을 반환
```


### Hash Table

<div class="content-box">
Key, Value 를 대응하여 저장하는 데이터구조. 
요즘에는 HashMap을 많이 사용함. 
</div>

**Hashing** : Key를 특정 계산식에 넣어 나온결과를 사용하여 값에 접근

**Hash-Collision** : HashTable의 같은 공간에 서로다른 값을 저장하여 하는 경우를 말함. 해결방법으로는 개방주소법(Open Addressing)/분리연결법 (Separate Chaining) 이 있음. 

**[Open Addressing]**

|**개방주소법**|특징|기타|
|--|--|--|
|선형탐사법|빈 공간을 순차적으로 탐사| 일차군집화 발생|
|제곱탐사법|빈 공간을 n제곱만큼 간격을 두고 탐사| 이차군집화 발생|
|이중해싱|해싱함수를 이중으로 사용|선형/제곱탐사에 비해 골고루 분포됨|

**[Separate Chaining]**

|**분리연결법**|특징|기타|
|--|--|--|
|분리연결법|HashTable을 연결리스트로 구현, 충동 시 같은위치에 연결리스트 내 저장|-|

**[sample]**
```java
HashTable<String,Integer> hashTable = new HashTable<>();

.put(Object key, Object value)  //지정된 key-value 쌍을 해시 테이블에 추가 또는 value 대체
.get(Object key)                //지정된 key에 해당하는 value를 반환
.remove(Object key)             //지정된 key에 해당하는 key-value 쌍을 해시 테이블에서 삭제합니다.
.clear()                        //저장된 모든 key-value 쌍을 삭제
.size()                         //저장된 key-value 쌍의 개수를 반환
.isEmpty()                      //해시 테이블이 비어있는지 여부를 반환
.contains(Object value)         //지정된 value가 해시 테이블에 포함되어 있는지 여부를 반환
.containsKey(Object key)        //지정된 key가 해시 테이블에 포함되어 있는지 여부를 반환
.containsValue(Object value)    //지정된 value가 해시 테이블에 포함되어 있는지 여부를 반환
.elements()                     //해시 테이블에 저장된 모든 값을 Enumeration 객체로 반환
.keys()                         //해시 테이블에 저장된 모든 key를 Enumeration 객체로 반환
.keySet()                       //해시 테이블에 저장된 모든 key를 Set 객체로 반환
.values()                       //해시 테이블에 저장된 모든 value를 Collection 객체로 반환
```