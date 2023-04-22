---
title: Non-Linear Data Structure(2)
category: 1. DataStructure
order: 1
---

### **Graph**
~~~java
import java.util.*;

public class Graph {
    private int V; // 정점 개수
    private LinkedList<Integer>[] adjList; // 인접 리스트

    // 생성자
    public Graph(int V) {
        this.V = V;
        adjList = new LinkedList[V];
        for (int i = 0; i < V; i++) {
            adjList[i] = new LinkedList<>();
        }
    }

    // 간선 추가 메소드
    public void addEdge(int u, int v) {
        adjList[u].add(v);
        adjList[v].add(u); // 무방향 그래프일 경우 양방향으로 추가
    }

    // DFS 메소드
    public void DFS(int v, boolean[] visited) {
        visited[v] = true;
        System.out.print(v + " ");
        Iterator<Integer> i = adjList[v].listIterator();
        while (i.hasNext()) {
            int n = i.next();
            if (!visited[n]) {
                DFS(n, visited);
            }
        }
    }

    // BFS 메소드
    public void BFS(int s) {
        boolean[] visited = new boolean[V];
        LinkedList<Integer> queue = new LinkedList<>();
        visited[s] = true;
        queue.add(s);
        while (queue.size() != 0) {
            s = queue.poll();
            System.out.print(s + " ");
            Iterator<Integer> i = adjList[s].listIterator();
            while (i.hasNext()) {
                int n = i.next();
                if (!visited[n]) {
                    visited[n] = true;
                    queue.add(n);
                }
            }
        }
    }

    // 메인 메소드
    public static void main(String[] args) {
        Graph graph = new Graph(5);
        graph.addEdge(0, 1);
        graph.addEdge(0, 4);
        graph.addEdge(1, 2);
        graph.addEdge(1, 3);
        graph.addEdge(1, 4);
        graph.addEdge(2, 3);
        graph.addEdge(3, 4);

        boolean[] visited = new boolean[graph.V];
        System.out.print("DFS: ");
        graph.DFS(0, visited);

        System.out.print("\nBFS: ");
        graph.BFS(0);
    }
}

~~~

![](https://ifh.cc/g/3yPcWF.jpg)
![](https://ifh.cc/g/ODllcx.jpg)
![](https://ifh.cc/g/nl0GKO.jpg)
![](https://ifh.cc/g/lWP0jp.jpg)

