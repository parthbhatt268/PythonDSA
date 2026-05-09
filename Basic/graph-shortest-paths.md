# Shortest Path Algorithms

---

## Shortest Path — Overview

- In **shortest path** questions, we often use **BFS** when the graph is **unweighted** (or all edges have the same cost).
- We usually **avoid DFS** for shortest path because DFS may go deep down a long route first, exploring lots of unnecessary nodes before finding the best answer.

### When BFS works well

- **Undirected / Directed unweighted graph** (each edge cost is the same, usually treated as `1`)
- **BFS guarantees the shortest path** (fewest edges) because it explores in **layers** (distance `0`, then `1`, then `2`, ...).

### When BFS does NOT work

- **Weighted graphs** (edges have different costs)
- BFS explores by **number of edges**, not by **total weight**.
- So BFS might reach a node using a **heavier** path first, mark it visited, and then **block** a later path that is **cheaper** (lower total weight) but uses more edges.

### What to use instead for weighted graphs

- **Dijkstra's Algorithm**: for graphs with **non-negative** edge weights
  - Key idea: always expand the node with the **smallest known distance** next (using a priority queue).
  - Instead of a simple `visited`, you keep a `dist[]` array:
    - If you find a cheaper way to reach a node, update `dist[node]` and continue.
- **Bellman–Ford**: if the graph can have **negative** edge weights (slower, but handles negatives).

### About "DFS for weighted shortest path"

- You can try DFS, but you must avoid repeating too much work. That usually means:
  - Track the **current total cost** along the path, and
  - Only continue exploring from a node if the current cost is **smaller** than the best cost you've seen for that node so far.
- This idea is basically moving toward what Dijkstra does (but Dijkstra is the standard, efficient solution).

### Kind of problems solved

- Undirected graph not weighted
- Undirected Cyclic graph — weighted
- Directed Acyclic Graph (DAG) — topo sorted

---

## Shortest Path cannot be found in a graph with a negative cycle

Because in that case you will keep on roaming and every time you will get a smaller route.

**Negative cycle** means in a cycle the weight of all edges added together is negative.

---

## Dijkstra's Algorithm

**Undirected cyclic weighted graph** — TC: `O(V^2)` SC: `O(V)`  
(Can be applied in directed and undirected graph)

In simple terms: for each vertex, check every outgoing edge. Take the current distance to that node + the edge weight; if that total is smaller than the distance stored at the neighbor (which may be ∞ at first), update/replace it. To avoid extra work and cycles, keep an explored/visited array and skip vertices that are already visited.

### Steps

1. Select the vertex which is **not explored yet** and its dist is **minimum** among all the unexplored vertices.

2. **Relax the edges:**
   - Look at all the unexplored neighbours
   - `if ( dist[node] + weight < dist[neighbour] )` then `dist[neighbour] = dist[node] + weight`

---

### Implementation choices for Dijkstra (Array vs Min-Heap / Priority Queue)

In Dijkstra's algorithm, we must **repeatedly pick the unexplored vertex with the smallest distance**.  
If we do this using a simple **array**, each "find-min" step can be expensive, because we may need to scan all unexplored vertices every time.

So, a common improvement is to use a **Min-Heap / Priority Queue** to always get the smallest-distance vertex faster.

#### Trade-offs

**1) Using an Array**

- **Space Complexity (SC):** `O(V)`
- **Time Complexity (TC):** Slower overall because selecting the minimum distance vertex requires scanning.

**2) Using a Min-Heap / Priority Queue**

- **Time Complexity (TC):** Improved, because:
  - `extract-min` and `insert` are faster with a heap.
  - Overall complexity is typically written as `O(E log E)` (often also expressed as `O(E log V)`).
- **Space Complexity (SC):** Increases to about `O(V + E)` because:
  - The priority queue can contain **repeated entries for the same node** (due to multiple relaxations),
  - So the heap size can grow close to the number of edges in the worst case.

✅ **Summary:**  
Using a min-heap speeds up Dijkstra's algorithm, but it usually uses more memory than the array-based approach.

> **Note (why `log E` or `log V`?):**  
> The `log` comes from heap operations (`push`/`pop`) which take `O(log N)` where `N` is the heap size.
>
> - If your priority queue keeps **at most `V` vertices** (e.g., using _decrease-key_), then `N ≈ V` → `O(E log V)`.
> - If your implementation **allows duplicates** (common in practice: push a new `(dist, node)` instead of decrease-key), the heap can grow to `N ≈ E` → `O(E log E)`.  
>   Also, since `E ≤ V²`, `log E` and `log V` differ only by a small constant factor (solve using maths: take log on both sides, you will eventually be left with `ElogV`).

- **Conclusion:** if there is dense graph then use ARRAY, if we have sparse graph then use Heap/priority queue.
- **Drawback:** it won't work with negative weights.

![alt text](image-3.png)

---

## Bellman-Ford Algorithm — Only works with Directed Graph

- Can help you with **negative weights**
- Can help you **detect negative cycle** as well
- BUT can only be used in **directed graph**

Shortest Path is not possible in Undirected Negative graph as there will always be a negative cycle present.

### Purpose

- Computes **single-source shortest paths** in a weighted graph.
- Works even if some edges have **negative weights**.
- Detects **negative-weight cycles** (reachable from the source).

---

### Key Terms

#### Relaxation — basically if you find an even cheaper route, update it

For an edge `(u → v)` with weight `w(u,v)`:

- If `dist[u] + w(u,v) < dist[v]`  
  then update `dist[v] = dist[u] + w(u,v)`

This update is called **relaxing the edge**.

---

### Steps / Rules (Bellman–Ford)

#### Step 1: Relax all edges V − 1 times

- Let V = number of vertices.
- Repeat **V − 1** rounds:
  - For **every edge** `(u → v)`, apply the relaxation rule.

**Why V − 1 times?**

- Any shortest path (without cycles) can have at most **V − 1** edges.

---

#### Step 2: Relax all edges one more time (cycle detection)

- Do **one additional pass** over all edges:
  - If **any distance value changes**, then a **negative-weight cycle exists** (reachable from the source).
  - If **no distance changes**, then the distances are the **shortest paths from the source**.

---

### Decision Summary

- **Change in Step 2** → **Negative-weight cycle present**
- **No change in Step 2** → **Shortest paths found**

> Dijkstra uses **Vertex** for the algorithm  
> Bellman-Ford uses **Edge** for the algorithm

Last 10 min is worth watching — https://www.youtube.com/watch?v=6DCnv6Q3iwk&t=1494s

---

## Floyd-Warshall Algorithm

Used on **directed, positive or negative both**, and to find **multi-source shortest distance**.

**ALSO: diagonal element will mostly be ZERO. If it is NEGATIVE then a NEGATIVE CYCLE is PRESENT.**

### Main Purpose

- Computes **all-pairs shortest paths**: shortest distance from **every node to every other node**
  - e.g., `1 → 2`, `1 → 3`, `2 → 5`, etc., for all pairs.

---

### Weight / Graph Notes

- Works with **positive and negative edge weights**.
- Commonly used on **directed graphs** as well as undirected graphs (it works for both).
- Not valid for shortest paths if a **negative-weight cycle** exists (distances become undefined).

---

### Core Idea (Intermediate Vertex Method)

- Repeatedly improve distances by allowing paths that go through an intermediate node.
- Each vertex becomes the **intermediate** once.

For vertices `i, j, k`:

- Check whether going from `i` to `j` via `k` is shorter:  
  `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`

---

### Steps / Rules (Floyd–Warshall)

1. **Initialize distance matrix `dist`**
   - `dist[i][j]` = edge weight from `i` to `j` if edge exists, else `+∞`
   - `dist[i][i] = 0` for all `i`

2. **Run intermediate-node loops**
   - For each node `k` (as the intermediate)
     - For every pair `(i, j)`, update:  
       `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`
   - Intuition:
     - First allow paths that use node 1 as intermediate, then node 2, then node 3, etc.

---

### Detecting Negative Cycles

- After running the algorithm:
  - If **any diagonal entry becomes negative**, i.e., `dist[i][i] < 0`  
    then the graph contains a **negative-weight cycle** (reachable from `i`).
- If a negative cycle exists, **shortest paths are not well-defined** for affected vertices.

---

### Complexity

- **Time Complexity:** `O(V^3)`
- **Space Complexity:** `O(V^2)` for the distance matrix
  - Updates are often done **in-place** on the same matrix, but the matrix itself still requires `V^2` space.

> marshall — did the work on Transitive law  
> floyd — did the work on adding and updating the weight on the edges

---

## Time Complexity Comparison

### Dijkstra's Algorithm (single-source)

- **Sparse graph:** `O((V+E) log V) ≈ O(E log V)`
- **Dense graph (array / matrix style):** `O(V^2)`
- **All-pairs via repeating Dijkstra:** `O(V · (V+E) log V)`
  - If `E = Θ(V^2)`: becomes `O(V^3 log V)`

### Bellman–Ford (single-source)

- **General:** `O(VE)`
  - If `E = Θ(V^2)` (dense): becomes `O(V^3)`
- **All-pairs via repeating Bellman–Ford:** `O(V^2 E)`
  - If `E = Θ(V^2)`: becomes `O(V^4)`

### Floyd–Warshall (all-pairs)

- **Always:** `O(V^3)`
- Does **all-pairs shortest paths in one run**, which is why it can outperform "repeat single-source" approaches on dense graphs.

---

## Practical Thumb Rule

- **Need single-source shortest paths + negative edges or cycle detection?** → Bellman–Ford
- **Need single-source shortest paths + nonnegative edges, graph is sparse?** → Dijkstra
- **Need all-pairs shortest paths, especially on dense graphs?** → Floyd–Warshall
