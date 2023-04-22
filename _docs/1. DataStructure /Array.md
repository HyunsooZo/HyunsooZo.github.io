---
title: Linear Data Structure
category: 1. DataStructure
order: 1
---
### **선형자료구조란?** 


선형 자료구조(Linear Data Structure)란, 자료들이 일렬로 늘어선 형태로 구성되어 있는 자료구조를 말한다. 이
러한 자료구조는 각 원소들 간에 순서가 존재하며, 특정 원소에 접근하기 위해서는 그 원소 이전의 모든 원소들을 순차적으로 접근해야 한다.

대표적인 선형 자료구조로는 배열(Array), 연결 리스트(Linked List), 스택(Stack), 큐(Queue), 데크(Deque), 해시 테이블(Hash Table)이 있다. 

각각의 자료구조는 자료의 삽입, 삭제, 검색 등의 연산에 따라서 효율성이 다르므로 프로그래밍 시 적절한 자료구조를 사용하는 것이 중요하다.


### **선형자료구조의 시간복잡도**

| 자료구조 | 탐색 | 삽입 | 삭제 |
| -------- | ---- | ---- | ---- |
| 배열     | O(1) | O(n) | O(n) |
| 링크드 리스트 | O(n) | O(1) | O(1) |
| 스택     | O(n) | O(1) | O(1) |
| 해시 테이블  | O(1) | O(1) | O(1) |
| 큐      | O(n) | O(1) | O(1) |
| 데크     | O(n) | O(1) | O(1) |


### **Array**
```
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
![](https://ifh.cc/g/63da9k.jpg)


### **Linked List**
~~~
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

![](https://ifh.cc/g/Sfm4yH.jpg)


### **Stack**
~~~
//Stack creation
Stack<String> stack = new Stack<>();  

.empty()            // 스택이 비어있는지 확인
.peek()             // 스택의 맨 위에 있는 요소를 반환. 스택이 비어있을 경우 null을 반환
.pop()              // 스택의 맨 위에 있는 요소를 제거하고 반환. 스택이 비어있을 경우 EmptyStackException을 발생
.push(E item)       // 지정된 요소를 스택의 맨 위에 추가
.search(Object o)   // 지정된 요소(o)가 스택의 위치 반환. 가장 최근에 추가된 요소=1,스택에 없을 경우 -1을 반환합니다.

~~~
![](https://ifh.cc/g/QYDpQS.jpg)


### **Queue & Deque**
~~~
// new LinkedList<>() in General 
Queue<String> queue = new LinkedList<>();
Deque<String> deque = new LinkedList<>(); 

//Methods for Queue
.add(e)       // 지정된 요소(e)를 큐의 끝에 추가. 큐가 가득 차 있을 경우 IllegalStateException을 발생
.offer(e)     // 지정된 요소(e)를 큐의 끝에 추가. 큐가 가득 차 있을 경우 false를 반환
.poll()       // 큐의 맨 앞에 있는 요소를 제거하고 반환. 큐가 비어있을 경우 null을 반환
.element()    // 큐의 맨 앞에 있는 요소를 반환. 큐가 비어있을 경우 NoSuchElementException을 발생
.peek()       // 큐의 맨 앞에 있는 요소를 반환. 큐가 비어있을 경우 null을 반환.

//Methods for Deque
.addFirst(e)  // 지정된 요소(e)를 덱의 맨 앞에 추가. 덱이 가득 차 있을 경우 IllegalStateException을 발생
.addLast(e)   // 지정된 요소(e)를 덱의 맨 끝에 추가. 덱이 가득 차 있을 경우 IllegalStateException을 발생
.offerFirst(e)// 지정된 요소(e)를 덱의 맨 앞에 추가. 덱이 가득 차 있을 경우 false를 반환
.offerLast(e) // 지정된 요소(e)를 덱의 맨 끝에 추가. 덱이 가득 차 있을 경우 false를 반환
.removeFirst()// 덱의 맨 앞에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 NoSuchElementException을 발생
.removeLast() // 덱의 맨 끝에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 NoSuchElementException을 발생
.pollFirst()  // 덱의 맨 앞에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 null을 반환
.pollLast()   // 덱의 맨 끝에 있는 요소를 제거하고 반환. 덱이 비어있을 경우 null을 반환
~~~
![](https://ifh.cc/g/vPArx7.jpg)


### **Hash Table**
~~~
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
~~~
![](https://ifh.cc/g/1z1ov6.jpg)



