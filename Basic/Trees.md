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

## Cycle Detection in a **Directed** Graph (Only Directed)

Example (real-life style):  
Resource 1 is waiting for Resource 2, Resource 2 is waiting for Resource 3, and Resource 3 is waiting for Resource 1.  
That creates a **directed cycle**: **1 → 2 → 3 → 1**.

---

## Why the undirected logic doesn’t work here

In an **undirected graph**, we said:

> “If a neighbor is already visited and it’s not the parent, then cycle exists.”

That rule is **not valid for directed graphs**, because direction matters.

In a directed graph, you might reach a node that was visited before, but that **doesn’t always mean a cycle**.  
It could be a cross-edge pointing to a node that was already fully processed, and that’s fine.

So we need a new rule.

---

## Correct logic for directed graphs (DFS Stack / Path)

In a directed graph, a cycle exists if:

✅ **A node appears more than once in the same DFS path.**

That means: if during DFS, you find an edge to a node that is **already in the current recursion stack**, then cycle is present.

This “current recursion stack” is called:

- **PATH**, or
- **DFS Stack**, or
- **Recursion Stack**

### Key idea

- When you go deeper in DFS, mark the node as `inPath = true`
- When you backtrack (return), set `inPath = false`  
  (this is what you meant by “remove the 1 from the visited array once we trace back”)

---

## Optimized version (2 arrays)

To make it faster, we usually keep **two arrays**:

1. **visited[]**  
   → means “this node is completely processed already”

2. **inPath[]** (or `dfsStack[]`)  
   → means “this node is currently in the DFS path”

### Flow:

When DFS reaches a node `u`:

1. If `inPath[u] == true`  
   → ✅ cycle found (because u appeared again in the same path)

2. Else if `visited[u] == true`  
   → skip it (we already checked this node’s paths before, no need to repeat)

3. Else
   - mark `visited[u] = true`
   - mark `inPath[u] = true`
   - DFS all neighbors
   - after done, mark `inPath[u] = false` (backtracking step)

This extra `visited[]` improves time complexity because you don’t re-check finished parts of the graph again.

---

## Another way (Kahn’s Algorithm / Topological Sort trick)

We know:
✅ Topological sorting works only for **DAG**.

So if we run **Kahn’s Algorithm (BFS topological sort)** on a directed graph:

- If the graph is a DAG → we will output **all nodes**
- If the graph has a cycle → we will output **fewer nodes than total**

Why fewer?  
Because nodes inside the cycle never reach **in-degree = 0**, so they never enter the QUEUE.

So:
✅ **If result size < number of nodes THEN the cycle EXISTS**

---

## Bipartite Graph (Simple Idea) — 2-Coloring Algorithm

A **bipartite graph** is a graph where you can split all nodes into **two groups** such that:
✅ every edge connects a node from Group 1 to a node from Group 2  
(and **no edge** connects two nodes inside the same group)

The easiest way to check bipartite is using the **2-coloring algorithm**.

---

## Key finding (Cycles rule)

- If the graph contains an **odd-length cycle**, then ❌ it is **NOT bipartite**
- If all cycles are **even-length** (or there are no cycles), then ✅ it **CAN be bipartite**

Reason (simple):  
In an odd cycle, when you alternate colors around the cycle, you eventually get forced to give the same color to two connected nodes.

---

## BFS-based 2-Coloring (most common)

We mostly use **BFS** for this.

### Setup

- Keep a `color[]` array:
  - `-1` = not colored yet
  - `0` and `1` = the two colors (two sets)

### Algorithm

For every node (because the graph might be disconnected):

1. If the node is not colored, assign it a color (say `0`) and push it into the queue.
2. While queue is not empty:
   - pop a node `u`
   - for every neighbor `v`:
     - if `v` is not colored → color it with the opposite color: `1 - color[u]`, and push to queue
     - else if `color[v] == color[u]` → ❌ conflict → graph is **not bipartite**

If BFS finishes without conflict, ✅ the graph is bipartite.

---

### Note

This is basically the same BFS “level-by-level” logic:

- nodes at alternating levels get alternating colors
- odd cycle breaks that rule and causes a color clash

USE case:
![alt text](image-2.png)

---

Exmaple Graph question are

1. Numebr of Island
2. Covid spread (the hostipal one where 2 -> already covid, 1-> normal pateint, 0->no one in the room) / Similar question Rotten Oranges
3. Replace 0's with X's and rules was 0 should be surrounded by X from all side and tehen you return the result 2D array

---

# 🌲 **TREE**

- For a tree with **n nodes**, there are **n−1 edges**; **no loops**.
- There is always a **root**, and each child has **one parent** only.
- **Every tree is a graph**, but **not every graph is a tree**.
