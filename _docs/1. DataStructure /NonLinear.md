---
title: Non-Linear Data Structure(1)
category: 1. DataStructure
order: 1
---
### **비선형자료구조란?** 

비선형 자료구조는 데이터 항목들이 계층적으로 조직되어 있는 자료구조를 의미한다. 이러한 자료구조에서는 각 항목들이 다른 항목들과 서로 다른 관계를 가지고 있을 수 있으며, 계층이나 망 형태로 구성되어 있다.

대표적인 비선형 자료구조로는 트리(Tree), 그래프(Graph), 힙(Heap) 등이 있으며 이들 자료구조는 데이터 항목들 간에 부모-자식, 연결된 노드-간선 등의 관계를 가지고 있으며, 이러한 관계를 이용해 데이터를 효율적으로 탐색하고 저장할 수 있다.


### **Tree - BFS ,  DFS**
~~~java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;

public class Tree {
    Node root;

    static class Node {
        int data;
        ArrayList<Node> children;

        public Node(int data) {
            this.data = data;
            children = new ArrayList<Node>();
        }
    }

    public Tree() {
        root = null;
    }

    public void add(int parentData, int data) {
        Node newNode = new Node(data);
        if (root == null) {
            root = newNode;
            return;
        }
        ArrayList<Node> queue = new ArrayList<Node>();
        queue.add(root);
        while (!queue.isEmpty()) {
            Node node = queue.remove(0);
            if (node.data == parentData) {
                node.children.add(newNode);
                return;
            }
            queue.addAll(node.children);
        }
    }

    public void dfs() {
        if (root == null) return;
        dfsRecursive(root);
    }

    public void dfsRecursive(Node node) {
        System.out.print(node.data + " ");
        for (Node child : node.children) {
            dfsRecursive(child);
        }
    }

    public void bfs() {
        if (root == null) return;
        Queue<Node> queue = new LinkedList<Node>();
        queue.add(root);
        while (!queue.isEmpty()) {
            Node node = queue.poll();
            System.out.print(node.data + " ");
            for (Node child : node.children) {
                queue.add(child);
            }
        }
    }

    public static void main(String[] args) {
        Tree tree = new Tree();
        tree.add(1, 2);
        tree.add(1, 3);
        tree.add(2, 4);
        tree.add(2, 5);
        tree.add(3, 6);
        tree.add(3, 7);
        System.out.println("DFS: ");
        tree.dfs(); // 1 2 4 5 3 6 7
        System.out.println("\nBFS: ");
        tree.bfs(); // 1 2 3 4 5 6 7
    }
}

~~~

![](https://ifh.cc/g/Wbr0QB.jpg)
![](https://ifh.cc/g/l3x6fm.jpg)
![](https://ifh.cc/g/vHJ1xH.jpg)
![](https://ifh.cc/g/yolPZX.jpg)




### **BinarySearchTree**
~~~java
//BinarySearch Tree 
public class Node {
    int data;
    Node left;
    Node right;

    public Node(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

public class BinarySearchTree {
    Node root;

    public BinarySearchTree() {
        root = null;
    }

    public void insert(int data) {
        root = insertRecursive(root, data);
    }

    public Node insertRecursive(Node root, int data) {
        if (root == null) {
            root = new Node(data);
            return root;
        }
        if (data < root.data)
            root.left = insertRecursive(root.left, data);
        else if (data > root.data)
            root.right = insertRecursive(root.right, data);

        return root;
    }

    public void inorder() {
        inorderRecursive(root);
    }

    public void inorderRecursive(Node root) {
        if (root != null) {
            inorderRecursive(root.left);
            System.out.print(root.data + " ");
            inorderRecursive(root.right);
        }
    }

    public static void main(String[] args) {
        BinarySearchTree bst = new BinarySearchTree();
        bst.insert(50);
        bst.insert(30);
        bst.insert(20);
        bst.insert(40);
        bst.insert(70);
        bst.insert(60);
        bst.insert(80);

        bst.inorder(); // 20 30 40 50 60 70 80
    }
}
~~~
### **+ TreeSet Class Java provides itself**
~~~java
import java.util.TreeSet;

public class BinarySearchTreeExample {
    public static void main(String[] args) {
        TreeSet<Integer> treeSet = new TreeSet<>();

        treeSet.add(50);
        treeSet.add(30);
        treeSet.add(20);
        treeSet.add(40);
        treeSet.add(70);
        treeSet.add(60);
        treeSet.add(80);

        System.out.println(treeSet.contains(40)); // true
        System.out.println(treeSet.contains(90)); // false
    }
}
~~~
![](https://ifh.cc/g/KxTy0y.jpg)
![](https://ifh.cc/g/kOSrMM.jpg)
![](https://ifh.cc/g/QjcVRd.jpg)
![](https://ifh.cc/g/DhGJDa.jpg)
![](https://ifh.cc/g/m0WSLL.jpg)
![](https://ifh.cc/g/VXtYp3.jpg)
![](https://ifh.cc/g/QwaoR6.jpg)
![](https://ifh.cc/g/VXtYp3.jpg)
![](https://ifh.cc/g/VXtYp3.jpg)
![](https://ifh.cc/g/VXtYp3.jpg)
![](https://ifh.cc/g/VXtYp3.jpg)