# Representation of Graph

## Introduction

A graph is a non-linear data structure consisting of a set of vertices (nodes) and edges connecting those vertices.

A graph is represented as:

[
G = (V, E)
]

Where:

* **V** = Set of Vertices
* **E** = Set of Edges

Example:

Vertices:

```text
V = {0, 1, 2, 3}
```

Edges:

```text
E = {(0,1), (0,2), (1,2), (2,3)}
```

Graph:

```text
    0
   / \
  1---2
       \
        3
```

---

# Types of Graphs

## Undirected Graph

In an undirected graph, edges have no direction.

```text
0 ----- 1
 \     /
   \ /
    2
```

If there is an edge between 0 and 1, then:

```text
0 → 1
1 → 0
```

Both directions are allowed.

---

## Directed Graph

In a directed graph, edges have a specific direction.

```text
0 → 1
↓
2 → 3
```

If there is an edge:

```text
0 → 1
```

it does not imply:

```text
1 → 0
```

---

## Weighted Graph

Edges contain weights or costs.

```text
0 --5-- 1

0 --2-- 2

1 --3-- 2
```

Here:

```text
Weight(0,1) = 5
Weight(0,2) = 2
Weight(1,2) = 3
```

---

# Graph Representation Methods

There are three major ways to represent a graph:

1. Adjacency Matrix
2. Adjacency List
3. Edge List

---

# 1. Adjacency Matrix

An adjacency matrix is a 2D matrix of size:

```text
V × V
```

where:

```text
matrix[i][j] = 1
```

if an edge exists between vertices i and j.

Otherwise:

```text
matrix[i][j] = 0
```

Example:

```text
    0
   / \
  1---2
       \
        3
```

Matrix:

```text
     0 1 2 3

0 -> 0 1 1 0
1 -> 1 0 1 0
2 -> 1 1 0 1
3 -> 0 0 1 0
```

Java Code:

```java
int[][] graph = {
    {0,1,1,0},
    {1,0,1,0},
    {1,1,0,1},
    {0,0,1,0}
};
```

<img width="688" height="483" alt="image" src="https://github.com/user-attachments/assets/9c4ddf48-ba2b-43da-bb66-104367778da3" />



### Advantages

* Easy edge lookup.
* Simple implementation.
* Useful for dense graphs.

### Disadvantages

* Consumes large memory.
* Not suitable for sparse graphs.

### Complexity

| Operation   | Complexity |
| ----------- | ---------- |
| Add Edge    | O(1)       |
| Remove Edge | O(1)       |
| Check Edge  | O(1)       |
| Space       | O(V²)      |

---

# 2. Adjacency List

In an adjacency list, each vertex stores a list of its neighbors.

Example:

```text
0 → 1 → 2

1 → 0 → 2

2 → 0 → 1 → 3

3 → 2
```

Java Implementation:

```java
import java.util.*;

ArrayList<ArrayList<Integer>> adj =
        new ArrayList<>();

for(int i=0;i<4;i++)
    adj.add(new ArrayList<>());

adj.get(0).add(1);
adj.get(1).add(0);

adj.get(0).add(2);
adj.get(2).add(0);

adj.get(1).add(2);
adj.get(2).add(1);

adj.get(2).add(3);
adj.get(3).add(2);
```
<img width="684" height="564" alt="image" src="https://github.com/user-attachments/assets/e76b810b-8646-4213-9047-f6d0bce8588a" />

### Advantages

* Memory efficient.
* Preferred for sparse graphs.
* Used in almost all graph algorithms.

### Disadvantages

* Edge lookup is slower than adjacency matrix.

### Complexity

| Operation          | Complexity |
| ------------------ | ---------- |
| Add Edge           | O(1)       |
| Traverse Neighbors | O(Degree)  |
| Space              | O(V + E)   |

---

# 3. Edge List

In an edge list, only edges are stored.

Example:

```text
(0,1)

(0,2)

(1,2)

(2,3)
```

Java Implementation:

```java
class Edge{
    int source;
    int destination;

    Edge(int source,int destination){
        this.source = source;
        this.destination = destination;
    }
}
```

```java
ArrayList<Edge> edges =
        new ArrayList<>();

edges.add(new Edge(0,1));
edges.add(new Edge(0,2));
edges.add(new Edge(1,2));
edges.add(new Edge(2,3));
```

### Advantages

* Easy to store.
* Useful in Kruskal's Algorithm.
* Useful in Bellman-Ford Algorithm.

### Disadvantages

* Neighbor traversal is expensive.

### Complexity

| Operation  | Complexity |
| ---------- | ---------- |
| Store Edge | O(1)       |
| Space      | O(E)       |

---

# Representation of Weighted Graph

Example:

```text
0 --5-- 1

0 --2-- 2

1 --3-- 2
```

Adjacency List Representation:

```text
0 → (1,5), (2,2)

1 → (0,5), (2,3)

2 → (0,2), (1,3)
```

Java Implementation:

```java
class Pair{
    int node;
    int weight;

    Pair(int node,int weight){
        this.node = node;
        this.weight = weight;
    }
}
```

```java
ArrayList<ArrayList<Pair>> adj =
        new ArrayList<>();
```

---

# Comparison of Graph Representations

| Feature     | Adjacency Matrix | Adjacency List | Edge List             |
| ----------- | ---------------- | -------------- | --------------------- |
| Space       | O(V²)            | O(V+E)         | O(E)                  |
| Edge Search | O(1)             | O(Degree)      | O(E)                  |
| Add Edge    | O(1)             | O(1)           | O(1)                  |
| Best For    | Dense Graphs     | Sparse Graphs  | Edge-Based Algorithms |

---

# Applications

### Adjacency Matrix

* Dense Graphs
* Network Connectivity
* Floyd-Warshall Algorithm

### Adjacency List

* BFS
* DFS
* Dijkstra Algorithm
* Prim Algorithm
* Topological Sort
* Tarjan Algorithm
* Kosaraju Algorithm

### Edge List

* Kruskal Algorithm
* Bellman-Ford Algorithm

---

# Key Interview Questions

### Why is Adjacency List preferred?

Because it requires:

```text
O(V + E)
```

space and is efficient for sparse graphs.

---

### When should Adjacency Matrix be used?

When the graph is dense and frequent edge lookups are required.

---

### Which representation is most commonly used?

Adjacency List.

---

### Which representation is used in Kruskal Algorithm?

Edge List.

---

# Conclusion

Graph representation is the foundation of all graph algorithms. Among all representations, Adjacency List is the most widely used because it provides an optimal balance between memory usage and traversal efficiency. Understanding graph representation thoroughly is essential before studying BFS, DFS, Shortest Path Algorithms, Minimum Spanning Trees, and Advanced Graph Theory.
