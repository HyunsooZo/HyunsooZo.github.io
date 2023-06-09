---
title: Non-Linear Data Structure
category: DataStructures
order: 1
---
### Non-Linear Data Structure? 

비선형 자료구조는 데이터 항목들이 계층적으로 조직되어 있는 자료구조를 의미한다. 이러한 자료구조에서는 각 항목들이 다른 항목들과 서로 다른 관계를 가지고 있을 수 있으며, 계층이나 망 형태로 구성되어 있다.

대표적인 비선형 자료구조로는 트리(Tree), 그래프(Graph), 힙(Heap) 등이 있으며 이들 자료구조는 데이터 항목들 간에 부모-자식, 연결된 노드-간선 등의 관계를 가지고 있으며, 이러한 관계를 이용해 데이터를 효율적으로 탐색하고 저장할 수 있다.


### Tree 

**[Tree]**

<div class="content-box">
· Node와 Link로 구성된 자료구조 (그래프의 일종이며 Acyclic)<br>
· 계층적 구조를 나타낼 때 유용 (folder..조직도..등)<br>
· 하나의 노드에서 다른 노드로 이동하는 경로는 유일함. <br>
· 노드가 N개인 트리의 Edge수는 N-1개.<br>
· 모든 노드는 서로 연결되어 있음. <br>
· 하나의 Edge를 끊으면 두개의 Sub Tree로 분리된다. 
</div>

**[Terms]**

|Term|Description|
|--|--|
|Node|트리구조의 Data값을 담는 단위|
|Edge|노드간의 연결선|
|Root Node|부모가 없는 최상위 노드|
|Leaf Node|자식이 없는 노드|
|Parent|연결된 두 노드 중 상위노드|
|Child|연결된 두 노드 중 하위노드|
|Sibling|같은 부모를 가지는 노드|
|Depth|Root 에서 어떤 노드까지의 가지 수|
|Level|특정 길이를 가지는 노드 집합|
|Height|트리의 가장 큰 Level값|
|Size|자신을 포함한 자식노드의 수|
|Degree|각 노드가 가진 가지의 수 |
|Degree of Tree|트리의 최대 차수|

**[Binary Tree]**

· 이진트리의 각 노드는 최대 2개의 자식을 가질 수 있다. 

· 자식노드를 좌/우로 구분한다. 

**[Perfect Binary Tree (포화 이진트리)]**

· 모든 level에 노드들이 꽉 채워져있다. 

**[Type of Tree]**
```s 
#[Perfect Binary Tree]            #[Complete Binary Tree]
#[포화 이진트리]                     #[완전 이진트리]   
#모든 level에 노드들이 꽉 채워져있다.    #마지막 레벨을 제외한 모든 노드들이 채워진 트리  

          A                                  A
       B     C                            B     C
     D   E F   G                        D   E F   
```
```s 
#[Full Binary Tree]               #[Skewed Binary Tree]
#[점 이진트리]                       #[편향 이진트리] 
# 모든 노드가 0 or 2개의 자식노드를 가짐.  #한쪽으로 기울어진 트리  

          A                                  A
       B     C                            B     
     D   E                              C      
```
```s 
#[Balanced Binary Tree]               
#[균형 이진트리]          
# 좌우 서브노트 트리높이 차가 1이하            

    A        A           A                
  B        B   C       B   D              
                     C
```

**[Tree Traversal]**

<div class="content-box">
· 모든노드를 누락/중복하지 않고 방문하는 연산<br>
· 순회의 종류는 4가지가 존재. (다음페이지에 코드 작성 예정)
</div>

  `DFS` {Pre-Order , In-Order , Post-Order}
  `BFS` {Level-Order}. 


### Binary Tree BFS / DFS

 **[임의의 Node Class]**

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```

**[DFS] : Pre-Order**
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> answer = new ArrayList<>();
        actualTraversal(answer,root);
        return answer;
    }
    public static void actualTraversal(List<Integer> list,TreeNode root){
        if(root == null) return;
        
        list.add(root.val);               // 탐색 위치  
        actualTraversal(list,root.left);
        actualTraversal(list,root.right);
    }
}
```
**[DFS] : In-Order**
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> answer = new ArrayList<>();
        recursive(root,answer);
        return answer;
    }
    public static void recursive(TreeNode root, List<Integer> answer){
        if(root==null) return;
        recursive(root.left,answer);
        answer.add(root.val);            // 탐색위치
        recursive(root.right,answer);
    }
}
```
**[DFS] : Post-Order**
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        checkCheck(list,root);
        return list;
    }
    public static void checkCheck(List<Integer> list,TreeNode root){
        if(root == null){
            return;
        }
    
        checkCheck(list,root.left);
        checkCheck(list,root.right);
        list.add(root.val);               // 탐색위치
    }
    
}
```


### BinarySearchTree

**Binary Search Tree**

<div class="content-box">
· Left Node < Parent Node<br>
· Right Node > Parent Node<br>
· 모든 키는 중복되지 않음. <br>
· 데이터가 정렬되어 있음 (중위순회)<br>
· 이진트리에 비해 탐색이 빠른편 (균형상태 O(logn) 불균형상태 O(N))
</div>

**Balanced Binary Search Tree**

· 모든 노드의 좌우 서브트리가 1이상 차이나지 않는 이진트리. 

· 노드의 삽입/삭제 시에도 균형을 유지함. 

· 대표적으로 AVL트리 , Red-Black Tree가 존재함. 

**1. AVL Tree**

· 노드 삽입/삭제 시 트리균형을 체크하고 유지함. 

· 각 노드의 Balance Factor 를 [-1,0,1]만 가지도록 하여 균형을 유지한다. 

++ Balance Factor : 왼쪽서브트리 높이 - 우측 서브트리높이

**Rebalancing of AVL Tree**

· 균형이 깨진 경우

**1.** BF가 양수이면 왼쪽 서브트리에 이슈

**2.** BF가 음수이면 왼쪽 서브트리에 이슈

**3.** 회전연산 진행 (LL , RR ,LR, RL)

![](https://ifh.cc/g/DhGJDa.jpg)

**Red-Black Tree**

[Red-Black Tree의 구조]

*1.* Root Node와 Leaf노드의 색은 `Black`

*2.* Red 색 Node의 자식노드는 `Black`(Double red 
isn't Allowed)

*3.* 모든 Leaf노드에서 Root노드까지 가능 경로의 Black노드 수는 같음. 

**+** 위 조건이 깨지는 상황에서 Rebalancing 진행. 

**+** Java에서 제공하는 TreeSet은 RedBlackTree를 구현한 라이브러리 임. 



### Gragh

**Graph**

<div class="content-box">
· Tree의 상위 개념이라고 볼 수 있다. <br>
· 정점과 간선으로 이루어진 자료구조 (Cyclic)<br>
· 지하철 노선도, 통신 네트워크 등에 사용. 
</div>

**[Term]**

|Term|Description|
|--|--|
|Vertex| 각 노드 (정점)|
|Edge| 링크|
|Adjacent Vertex|간선하나를 두고 바로 연결된 인접정점|
|Degree|무방향그래프에서 하나의 정점에 인접한 정점 수|
||(무방향) 그래프 모든 정점차수의 합 = 그래프 간선 수 *2|
|In-Degree| 방향그래프에서 외부에서 들어오는 간선 수|
|Out-Degree|방향그래프에서 외부로 나가는 간선 수|
|Path Length|경로구성에 사용된 간선 수|
|Simple Path|경로 중 반복되는 정점이 없는 경우|
|Cycle|단순 경로의 시작정점 = 끝정점|

**[Comparison of Tree and Graph]**

||Graph|Tree|
|--|--|--|
|개요|노드,간선으로 이루어진 자료구조| 그래프의 한 종류|
|방향성|방향그래프, 무방향그래프| 방향그래프|
|사이클|Cyclic| Acyclic|
|모델|네트워크 모델|계층 모델|
|루트 노드|x|최상위 노드|
|부모 자식|x|인접 상-하위 노드|
|간선 수|그래프에 따라 다름|N(노드수)-1개|
|순회|DFS , BFS|DFS(Pre/In/Post Order),BFS(LevelOrder)|
|경로|2개이상의 경로 가능| 두 노드간의 경로는 1개|

**[그래프의 종류]**

![](https://ifh.cc/g/ODllcx.jpg)

**[그래프의 구현]**

<div class="content-box">
· 인접리스트를 활용 (Adjacent List) : 연결리스트 사용<br>
· 메모리 사용량이 상대적으로 적고, 노드의 추가 삭제가 빠르나 간선정보 확인시 시간이 걸림
</div>

```
 A--------B            Arr    LinkedList
 |      / |            |A|-->|B|*|-->|C|*| 
 |    /   |            |B|-->|A|*|-->|C|*|-->|D|*|  
 |  /     |            |C|-->|A|*|-->|B|*|-->|D|*|  
 C--------D            |D|-->|B|*|-->|C|*| 
```

`인접행렬` -> 노드의 개수가 적고 간선의 수가 많을 때 유리 (밀집그래프)

`인접리스트` -> 노드의 개수가 많고 간선의 수가 적을 때 유리 (희소그래프)

**[인접행렬 과 인접리스트]**

||인접행렬|인접리스트|
|--|--|--|
|특정 간선 검색|O(1)|O(degree(V)|
|정점의 차수 계산|O(N)|O(degree(V)|
|전체노드 탐색|O(N^2)|O(E)|
|메모리|N*N | N+E|

### BFS , DFS For Graph

**[DFS]**: 각 노드 방문 여부를 체크할 `Array`+`Stack` 이용하여 구현

**[BFS]**:각 노드 방문 여부를 체크할 `Array`+`Queue` 이용하여 구현

```java
// 그래프 클래스
class Graph {
    private int V; // 노드의 수
    private LinkedList<Integer>[] adj; // 인접 리스트

    // 생성자
    Graph(int v) {
        V = v;
        adj = new LinkedList[v];
        for (int i = 0; i < v; ++i) {
            adj[i] = new LinkedList<>();
        }
    }

    // 노드와 연결된 간선 추가
    void addEdge(int v, int w) {
        adj[v].add(w);
    }

    // DFS 알고리즘
    void DFS(int v) {
        boolean[] visited = new boolean[V]; // 노드의 방문 여부 저장
        DFSUtil(v, visited); // DFSUtil 호출
    }

    void DFSUtil(int v, boolean[] visited) {
        visited[v] = true; // 현재 노드 방문 표시
        System.out.print(v + " "); // 현재 노드 출력

        Iterator<Integer> i = adj[v].listIterator();
        while (i.hasNext()) {
            int n = i.next();
            if (!visited[n]) { // 방문하지 않은 인접 노드면
                DFSUtil(n, visited); // 재귀 호출
            }
        }
    }

    // BFS 알고리즘
    void BFS(int v) {
        boolean[] visited = new boolean[V]; // 노드의 방문 여부 저장

        LinkedList<Integer> queue = new LinkedList<>(); // 큐 생성

        visited[v] = true; // 시작 노드 방문 표시
        queue.add(v); // 큐에 추가

        while (queue.size() != 0) {
            v = queue.poll(); // 큐에서 첫번째 노드 추출
            System.out.print(v + " "); // 현재 노드 출력

            Iterator<Integer> i = adj[v].listIterator();
            while (i.hasNext()) {
                int n = i.next();
                if (!visited[n]) { // 방문하지 않은 인접 노드면
                    visited[n] = true; // 방문 표시
                    queue.add(n); // 큐에 추가
                }
            }
        }
    }
}
```

