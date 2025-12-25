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
- **Bounding function** (mostly used in BFS)

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

## BFS (Breadth First Search) Traversal

### Intuition

**BFS is like spreading a virus 🦠**  
One node spreads to all its neighbors, then those neighbors spread to their neighbors, and so on — **level by level**.

---

### Why We Use a Queue for BFS

- BFS follows **First In, First Out (FIFO)**.

### Steps

1. Start from a node and mark it as visited.
2. Add the node to the **queue**.
3. Remove the **front element** from the queue.
4. Visit all its **unvisited adjacent nodes**.
5. Add those adjacent nodes to the **end of the queue**.
6. Repeat until the **queue is empty**.

### Key Property

- BFS explores nodes **level by level**.
- It is commonly used to find the **shortest path in an unweighted graph**.

### Application of BFS

1. Shortest Path in an
2. unweighted graph
3. Cycle Detection
4. Crawlers in Search Engine
5. Social Networking Search
6. In Garbage Collection
7. Broadcasting

### Sample Question

1. BFS of Graph (Solved soln in folder)

---

### **Graph Representation**

**Two common ways TO REPRESENT GRAPH:**

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

### Question

1. BFS Traversal in Graph
2. DFS Traversal in Graph
3. Cycle detection in Graph using BFS/DFS

---

## Cycle Detection in an Undirected Graph

There are two methods to solve this: **DFS** and **BFS**.

### Logic

The main idea is:  
If a node gets **visited again** at any point during DFS/BFS, we might think there’s a cycle.

But in an **undirected graph**, this can trick us.

Example: if we only have two nodes **0 ↔ 1**, then:

- from 0 we go to 1
- from 1 we can go back to 0 (because it’s undirected)

So it _looks_ like we visited a node twice, but that’s **not a real cycle**.

That’s why we add one condition:  
✅ **Ignore the edge back to the parent node.**

#### --> In DFS, 3 cases can happen

When you're at a node and checking one of its neighbors:

1. **The neighbor is the parent**  
   → ignore it (skip checking cycle for this edge)

2. **The neighbor is already visited (and not the parent)**  
   → declare **cycle present**

3. **The neighbor is not visited yet**  
   → visit that node and repeat the same process

---

## Topological sorting

## 1. Using DFS

Usecase: Where lets say process A has to be done beofre process B and process c has to be done bofre process B etc (like wirting html brofre csss and wirting css/html beofre writing JS)

You only do **topological sorting** in a **DAG (Directed Acyclic Graph)**, because:

- If the graph is **not directed**, you don’t have a clear “who comes before whom” direction.
- If the graph is **cyclic**, you get stuck in a dead loop.

Topological sorting means: if we have a directed graph, we want to sort the nodes into a single line (an order) showing **who comes before whom**.

For example, you might have rules like:

- **A should come before B**
- **B should come before C and D**

So one valid topological order could be: **A → C → D → B** (the idea is just to represent the order in one line).

That’s why we need:

- a **directed** graph (to know who comes before whom), and
- an **acyclic** graph (because if it’s cyclic, we are stuck in a loop like:  
  **A comes before B, B comes before C, but C comes before A**, which is impossible to satisfy).

A simple way to think about solving topological sorting is this line:

> **“A particular node will only go into the stack when all the nodes it depends on are already in the stack.”**

And yes, we use a **stack** here because it’s one of the easiest ways to handle the ordering in these cases.

## 2. Using BFS - Kahn’s Algorithm

--> (BFS method for Topological Sorting)

This is the **second way** to do topological sorting: using **BFS with Kahn’s Algorithm**.

### Core idea

We keep finding nodes that have **no parents** (meaning: **no incoming edges**).  
We add those nodes to the answer, then “remove” them from the graph, and repeat.

To do this properly, we need to understand one key term:

- **Indegree** = number of **incoming edges** (how many parents a node has)
- **Outdegree** = number of **outgoing edges** (how many children it points to)

---

### Step 1: Compute indegree for every node

The easy way is:

- Look at the **adjacency list**
- For every edge `u -> v`, increment `indegree[v]` by 1

So indegree is basically:  
✅ “How many times this node is reachable as a _neighbor_ from others.”

---

### Step 2: Push all indegree = 0 nodes into a queue

Since this is BFS, we use a **QUEUE**.

- Put every node whose **indegree is 0** into the queue
- These are the nodes with no parents, so they can safely come first

---

### Step 3: BFS process

While the queue is not empty:

1. Pop the front node `u`
2. Add `u` to the **answer**
3. For every neighbor `v` of `u`:
   - Decrease `indegree[v]` by 1 (because we are “removing” the edge `u -> v`)
   - If `indegree[v]` becomes 0, push `v` into the queue

---

### Why this works

Each time a node’s indegree becomes 0, it means:
✅ all of its parents have already been placed in the answer  
so it’s now safe to place it next.

---

### Extra note (important)

If at the end, the answer does **not** include all nodes, that means:
❌ the graph had a **cycle**, so topological sorting is not possible.

![alt text](image-1.png)

---

Cycle detetcion in ONLY Directed graph

exmaple is liek resourc1 can ask for resurce 2 and resource 2 can ask for resource 3 but reosuce 3 itlsef is wait for resource 1 to be free so here we ahve directed cyclid grpah

here our logoc we used in cycle detectin in undirected grpah, where we simple said if the node you are currently checking is aldresy visted and if it is not the parent then we have foudn a cycle. here in diretced grpah that wont work because condiion you mentioend aboev like see current node is alreaydy visted and and not parent event in that casein directed grpah that wont be a cycle since direction woudld be miss matched.

so teh new logic is that we will remove the 1 from the vissed aray once we tracse back the same route
if a node appear more than once in a path then we declare it cycle

you can call it PATH or DFS STACK

Also to optimse the execution you can do one mroe thing like also have avisted arrya and then do simple thing like keep a array named visted of all the existing visted node and then the flow is simple we will first check if teh current node is not in the PATH and if taht is faslse then we check if the current node is already visted list then we simply then ignoreing cbecking that same path again because we already vissted that path and we didnt foudn beofre so we could probaly skip it again.this additoinal visted array will help us increase the efficenlty TC

Very Imp there is other way to find cycle in directed groah as well
so since we knwo that we can only apply topological sort on DAG uing kahn's algo that is BFS way.
but if we apply kahn algo on a DCG dirceted cyclis graaph then it only be right since we will only get outut node in topiligical sorted way for tehnode wihch are not in the cylce in that grpah so you will see at teh edn teh result you get will have lesser node ans then actually node in the groaph

---

# 🌲 **TREE**

- For a tree with **n nodes**, there are **n−1 edges**; **no loops**.
- There is always a **root**, and each child has **one parent** only.
- **Every tree is a graph**, but **not every graph is a tree**.
