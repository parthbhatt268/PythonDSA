# 🌳 **Graph & Tree Notes**

---

## 🔁 **DFS and BFS**

- **DFS (Depth First Search)** → uses a **stack**, like **pre-order traversal** in a tree.
- **BFS (Breadth First Search)** → uses a **queue**, like **level-order traversal** in a tree.

---

## ⚡ **Articulation Point**

> An **articulation point** is a node in a graph which, if removed, breaks the graph into **two or more components**.  
> Think of it as a **single point of failure** in a system or network.

To avoid this, we use **bi-connected components** — effectively adding an **extra edge** so there are two different routes even if the articulation point is removed.

---

## 🔄 **Backtracking & Branch and Bound**

- **Backtracking (DFS)**
- **Branch and Bound (BFS)**
- **State space tree**
- **Bounding function**

Backtracking uses a **state-space diagram** (a tree-like structure) and is handy when we need **all possible ways** of something.

---

## 🎨 **M-Coloring Problems**

- **M-coloring decision problem**  
  Decide whether **3** (or **4**) colors are enough to color the graph so that **no adjacent nodes** share the same color.

- **M-coloring optimization problem**  
  Find the **minimum number of colors** required to color the graph with the same constraint.

---

## 🔁 **Hamiltonian Cycle**

A **Hamiltonian cycle** is a cycle in a graph where you **visit every node once** and **return to the starting node**.

> If the graph has an **articulation point** or a **pendant node** (child node), then a **Hamiltonian cycle is not possible**.

---

# 🕸️ **GRAPH**

### **Types of Graphs**

- **Directed**, **Undirected** graph
- **Cyclic** (even one cycle makes the whole graph cyclic), **Acyclic** graph
- **Directed cyclic** graph, **Directed acyclic graph (DAG)**
- **Connected** and **Disconnected** graph
  - In a **directed** graph, you might not be able to go in the reverse direction, which can make it **disconnected**.
- **Complete** graph — every node is connected to all other nodes
- **Weighted**, **Unweighted** graph

---

### **Graph Representation**

**Two common ways:**

1. **Adjacency Matrix**  
   Used when the graph is **dense** (close to complete).

   - **Time Complexity:** `O(V^2)`
   - **Space Complexity:** `O(V^2)`
   - Drawback: Uses extra space to represent **no edge** (e.g., storing `0`).

2. **Adjacency List**  
   Used when the graph is **sparse**.
   - **Time Complexity:** `O(V + E)`
   - **Space Complexity:** `O(V + E)`
   - In the **worst case** (complete graph), TC and SC approach `O(V^2)`.

![difference between adjacency matrix and adjacency list](image.png)

---

# 🌲 **TREE**

- For a tree with **n nodes**, there are **n−1 edges**; **no loops**.
- There is always a **root**, and each child has **one parent** only.
- **Every tree is a graph**, but **not every graph is a tree**.
