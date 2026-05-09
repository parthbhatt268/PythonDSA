# Advanced Graph — Euler, Hamiltonian, M-Coloring, Bridges, Articulation Points, SCC

---

## Euler Path

Analogy: picking up dustbins along a road — it is a path in a graph that visits every **EDGE exactly once**.

**EULER is for EDGE not VERTEX**

Maybe rule is that start from node which has highest in-degree (MAYBE).

### Conditions

1. Zero or two nodes can have **odd degree** and remaining nodes should have **even degree**.
2. All **non-zero nodes should be connected**.

---

## Euler Circuit

**Euler Circuit = Euler path + starting and ending NODE should be same**

Another thing: if a graph is an **Euler circuit** then in the graph if you start from **any node**, the Euler circuit will always exist.  
This is **not the same with Euler path**.

### Conditions

1. **All node degrees will be EVEN**
2. All edges should be part of **single component** OR all **non-zero degree nodes should be connected**

**EULER – EDGE – EVEN**

---

## For Solving Euler Circuit in Code

1. Find **degree of each node**
2. If **even one degree is odd → not an EC**
3. If **all are even then continue**
4. Apply **DFS from any non-zero degree node**

So you will have two arrays: **visited** and **indegree**

5. Now compare the two arrays: if there is an **indegree > 0** but the corresponding node in **visited is false**, then we simply say **no EC**
6. For any node that **does not have edge (indegree = 0)** we do **not need to check if it is visited**, we can simply **ignore them**

---

## Hamiltonian Path and Hamiltonian Cycle

- A **Hamiltonian Path** visits every **vertex (node) exactly once**; doesn't have to return to the start.
- A **Hamiltonian Cycle** visits every vertex exactly once and **returns to the starting node** — the last visited node must have a direct edge back to the starting node.

**Example to remember:** Think of a delivery person who wants to visit every city exactly once — that minimizes total travel time.

### Key Facts

- **IMP:** The choice of starting node can also decide whether a Hamiltonian path exists or not.
- If a Hamiltonian path exists, it does **NOT** mean a Hamiltonian cycle will also exist.
- BUT if a Hamiltonian cycle exists, then a Hamiltonian path is **guaranteed** to exist as well.

### Complexity

- **Hamiltonian Cycle Problem**: Determining whether a Hamiltonian cycle exists is **NP-complete**, and brute-force solutions can take **O(n!) (factorial time)**. AHHHHHHHHH :)

> If the graph has an **articulation point** or a **pendant node** (child node), then a **Hamiltonian cycle is not possible**.

---

## Graph Paths and Cycles — Comparison (Eulerian vs Hamiltonian)

| Type              | Visits           | Constraint                 |
| ----------------- | ---------------- | -------------------------- |
| Hamiltonian Path  | Each vertex once | Doesn't return to start    |
| Hamiltonian Cycle | Each vertex once | Returns to starting vertex |
| Eulerian Path     | Each edge once   | Doesn't return to start    |
| Eulerian Cycle    | Each edge once   | Returns to starting vertex |

### Examples

#### 1. Every Eulerian Circuit is **not** necessarily a Hamiltonian Cycle

Imagine a graph with 5 nodes arranged so that you can visit all 5 nodes once, forming a Hamiltonian cycle. However, while doing this you might have to travel over some edges more than once.  
This breaks the Eulerian rule of using **each edge exactly once**, so it would **not** be an Eulerian circuit.

#### 2. Every Eulerian Cycle is **not** necessarily a Hamiltonian Cycle

Imagine a graph where you visit **all edges exactly once** (Eulerian cycle), but there is a **separate node disconnected** from the main structure.

In that case:

- You covered all edges → satisfies Eulerian cycle.
- But you **missed one vertex** → not a Hamiltonian cycle.

### Eulerian Cycle Check — Much more efficient

Can be determined by checking the **degree of vertices** (all vertices must have even degree for a cycle).

---

## M-Coloring Problems

- **M-coloring decision problem**  
  Decide whether **3** (or **4**) colors are enough to color the graph so that **no adjacent nodes** share the same color.

- **M-coloring optimization problem**  
  Find the **minimum number of colors** required to color the graph with the same constraint.

---

## Bridge Detection in Graphs

> Finding edges whose removal increases the number of connected components.

**Tags:** Graph Theory · DFS · Tarjan's Algorithm · O(V + E)

**Bridge is for EDGE; Articulation Point is for identifying NODES.**

### What is a Bridge?

An edge `(u, v)` in an undirected graph is called a **bridge** if removing it disconnects the graph — i.e., the number of connected components increases. Bridges represent critical connections: roads, network links, or any path that has no alternate route.

---

### Key Parameters Per Node

| Parameter    | Description                                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------ |
| `disc[u]`    | **Discovery time** — the step at which node `u` was first visited in the DFS traversal.                      |
| `low[u]`     | **Low time** — the lowest discovery time reachable from the subtree rooted at `u`, including via back-edges. |
| `visited[u]` | Boolean flag to track whether node `u` has been visited in DFS.                                              |
| `parent[u]`  | The node from which `u` was reached in DFS. Used to avoid traversing the same edge backwards.                |

---

### Bridge Condition

> **An edge `(u, v)` is a bridge if `disc[u] < low[v]`.**

This means the subtree of `v` has no back-edge that can reach `u` or any ancestor of `u`. The only path back goes through the edge `(u, v)` itself.

---

### The 3 Cases During DFS

For each node `u` being processed, iterate over all its neighbors `v`:

#### Case 1 — Neighbor is the parent

```
if v == parent[u]  →  skip it
```

This is the edge we just came from. In an undirected graph, every edge appears twice in the adjacency list, so we must ignore this to avoid treating the parent edge as a back-edge.

**Action:** `continue` to next neighbor.

---

#### Case 2 — Neighbor is already visited (back-edge)

```
else if visited[v] == true  →  update low[u]
    low[u] = min(low[u], disc[v])
```

Node `v` is already visited and is an ancestor. We can reach `v`'s discovery time from `u`'s subtree.

> **Note:** We use `disc[v]` here, not `low[v]`. This keeps us honest — we only count actual ancestors, not paths that might loop through unrelated nodes.

**Action:** Update `low[u]` using `disc[v]`.

---

#### Case 3 — Neighbor is unvisited (tree edge)

```
else  →  recurse into v, then check bridge condition
    DFS(v, parent = u)
    low[u] = min(low[u], low[v])
    if disc[u] < low[v]  →  (u, v) is a bridge
```

Recursively DFS into `v`. After it returns, propagate the low value back up and immediately check the bridge condition.

**Action:** DFS → update `low[u]` → check bridge condition right away.

---

### The Critical Insight — It All Happens Simultaneously

> ⚠️ There is **no** "first pass to mark disc and low, then second pass to find bridges." Everything happens in a **single DFS**.

The moment the recursion returns from a child node `v` back to `u`, we immediately:

1. Update `low[u]`
2. Check if `(u, v)` is a bridge

...right there, before moving to the next neighbor.

Think of it as a **chain reaction**: as soon as DFS detects a back-edge (Case 2), that information — _"this subtree can reach an ancestor at time X"_ — propagates back up the call stack. Each returning frame updates its own `low` and checks its own edge. By the time DFS finishes, every bridge has already been found.

---

### Step-by-Step Flow

1. Start DFS from any unvisited node. Assign `disc[u] = low[u] = timer++` and mark it visited.
2. For each neighbor `v` of the current node `u`, apply the 3 cases above.
3. When DFS on child `v` returns, do `low[u] = min(low[u], low[v])` — the child might have found a path to an ancestor that `u` can now benefit from.
4. Immediately check: `if disc[u] < low[v]` → record edge `(u, v)` as a bridge.
5. Continue to `u`'s next neighbor. Repeat. Bridges accumulate in a result list as the DFS unwinds.

---

### Why `disc[u] < low[v]` Works

If `low[v]` is less than or equal to `disc[u]`, it means `v`'s subtree has a back-edge reaching `u` or higher — there's an alternate path. Removing `(u, v)` wouldn't disconnect anything.

But if `low[v] > disc[u]` (equivalently, `disc[u] < low[v]`), the subtree under `v` is completely isolated from `u`'s ancestors — the only lifeline is the edge itself. That's a bridge.

#### Why not `low[v] >= disc[u]`?

In bridge detection (unlike articulation points) we use **strict** less-than. If `low[v] == disc[u]`, it means `v` can reach exactly back to `u` — there's a cycle through `u`, so the edge is **not** a bridge. Only when `low[v] > disc[u]` is there truly no alternate path.

---

### Time & Space Complexity (Bridge Detection)

|           | Complexity | Reason                                                                                  |
| --------- | ---------- | --------------------------------------------------------------------------------------- |
| **Time**  | `O(V + E)` | Single DFS pass — every vertex and edge is visited exactly once.                        |
| **Space** | `O(V)`     | Arrays for `disc`, `low`, `visited`, `parent` — each size V. Plus O(V) recursion stack. |

---

### Quick Recap (Bridges)

- `disc[u]` — when was `u` first seen
- `low[u]` — earliest ancestor `u`'s subtree can reach
- **Bridge condition:** `disc[u] < low[v]` after DFS on child `v` returns
- **3 cases:** skip parent · update low on back-edge · recurse on unvisited then check bridge
- **One pass only** — discovery, low propagation, and bridge detection all happen in the same DFS unwind

---

## Articulation Points — Tarjan's Algorithm

> A node is an **articulation point** if removing it increases the number of connected components.

**Tags:** Graph Theory · DFS · Tarjan's Algorithm · O(V + E)

### Key Parameters

| Parameter    | Description                                                          |
| ------------ | -------------------------------------------------------------------- |
| `disc[u]`    | Discovery time — when node `u` was first visited.                    |
| `low[u]`     | Lowest discovery time reachable from `u`'s subtree (via back-edges). |
| `parent[u]`  | Node from which `u` was reached in DFS.                              |
| `visited[u]` | Whether `u` has been visited.                                        |

---

### The 2 Cases During DFS

For each node `u`, iterate over all neighbors `v`:

#### Case 1 — Neighbor already visited (back-edge)

```
low[u] = min(low[u], disc[v])
```

Use `disc[v]`, **not** `low[v]`.

> We want to know if `u` can reach back to that specific ancestor node `v`, not further beyond it. Using `low[v]` would allow jumping past `v` to its ancestors, which is incorrect for articulation point detection.

---

#### Case 2 — Neighbor not visited (tree edge)

```
DFS(v, parent = u)
low[u] = min(low[u], low[v])

if disc[u] <= low[v]  →  u is an articulation point
```

After DFS returns, propagate `low` upward. If `v`'s subtree cannot reach any ancestor of `u`, then removing `u` disconnects `v`.

> **Bridge uses `<`, articulation point uses `<=`** — if `low[v] == disc[u]`, `v` can only reach back to `u` itself. Removing `u` still cuts `v` off, so `u` is still an articulation point.

---

### The Parent Edge Case

Applying the above naively makes the **DFS root always appear as an articulation point**. Special rule needed:

```
if u is root:
    u is an articulation point  →  only if it has 2+ DFS children
```

- **1 child** — removing root just removes a dead-end. Not an articulation point.
- **2+ children** — those subtrees are only connected through root. Removing it splits them. Articulation point.

---

### Why `disc[u] <= low[v]` and not `<`

| `low[v]` vs `disc[u]` | Meaning                                                                     | Articulation Point? |
| --------------------- | --------------------------------------------------------------------------- | ------------------- |
| `low[v] < disc[u]`    | `v`'s subtree reaches an ancestor _above_ `u` — alternate path exists.      | No                  |
| `low[v] == disc[u]`   | `v`'s subtree can only reach back to `u` — removing `u` still cuts `v` off. | **Yes**             |
| `low[v] > disc[u]`    | `v`'s subtree can't reach `u` at all.                                       | **Yes**             |

Condition `disc[u] <= low[v]` catches both the last two cases.

---

### Bridge vs Articulation Point — Key Difference

|                               | Condition           | Back-edge update                |
| ----------------------------- | ------------------- | ------------------------------- |
| **Bridge** (edge)             | `disc[u] < low[v]`  | `low[u] = min(low[u], disc[v])` |
| **Articulation Point** (node) | `disc[u] <= low[v]` | `low[u] = min(low[u], disc[v])` |

Both use `disc[v]` (not `low[v]`) on back-edges — we care about reaching that specific ancestor, not jumping further up.

---

### Time & Space Complexity (Articulation Points)

|           | Complexity | Reason                                                           |
| --------- | ---------- | ---------------------------------------------------------------- |
| **Time**  | `O(V + E)` | Single DFS — every node and edge visited once.                   |
| **Space** | `O(V)`     | Arrays for `disc`, `low`, `visited`, `parent` + recursion stack. |

---

### ❌ Failed Hypothesis 1 — "If a neighbor is in the same cycle, the node can't be an articulation point"

**Why it sounds right:** If a node's neighbors are all part of the same cycle, removing it seems safe because the cycle provides alternate paths.

**Why it fails:** A node can be part of a cycle _and still_ be an articulation point — because it may also connect to a completely separate subtree that has no other way back.

**Example:**

```
1 - 2 - 3 - 1   (a cycle: 1, 2, 3)
        |
        4
```

Node `3` is part of the cycle `1-2-3`. But node `4` is only connected through `3`. Removing `3` disconnects `4` entirely. So `3` **is** an articulation point — even though it sits inside a cycle.

> The cycle only protects the nodes within it. Any dangling subtree hanging off a cycle node is still vulnerable.

---

### ❌ Failed Hypothesis 2 — "Find all bridges first, then their endpoints are articulation points"

**Why it sounds right:** A bridge is a critical edge, so naturally both nodes on either end seem critical too.

**Why it fails — two reasons:**

1. **A node can be an articulation point without being part of any bridge.** If a node sits at the centre of two cycles, removing it disconnects them — but all its edges are part of cycles, so none of them are bridges.

2. **Not every bridge endpoint is an articulation point.** A leaf node (degree 1) has one bridge edge — but removing it only removes itself, it doesn't split the graph. It is not an articulation point.

**Example for point 1:**

```
1 - 2 - 3
|       |
4 - 5 - 6
    |
    7 - 8 - 9
        |   |
        10-11
```

Node `5` connects two cycles (top square and bottom square). No edge from `5` is a bridge. But removing `5` disconnects the two halves. Bridge-first approach misses `5` entirely.

**Example for point 2:**

```
1 - 2 - 3
```

Edge `(1, 2)` is a bridge. Node `1` is a leaf — removing it doesn't disconnect anything. Node `1` is **not** an articulation point despite being a bridge endpoint.

> The bridge-first approach is both incomplete (misses some articulation points) and over-eager (incorrectly flags some bridge endpoints). Tarjan's algorithm handles all cases correctly in one pass.

---

### Quick Recap (Articulation Points)

- Back-edge → `low[u] = min(low[u], disc[v])`
- Tree edge → DFS, then `low[u] = min(low[u], low[v])`, then check `disc[u] <= low[v]`
- Root is special → articulation point only if it has **2+ DFS children**
- One pass only — everything happens as DFS unwinds

---

## SCC — Strongly Connected Component

A **Strongly Connected Component** is a maximal group of nodes where every node can reach every other node.

- **Correct:** If a component is an SCC, it remains an SCC even when we reverse the direction of all edges in that component.
- **Wrong:** Only two nodes forming a cycle make an SCC — incorrect, an SCC can have any number of mutually reachable nodes.
- **Correct:** An SCC can contain one or more cycles.

---

## Kosaraju's Algorithm

1. **Topological sort using DFS** — Do NOT use Kahn's (BFS); Kahn's stops and returns nothing when it detects a cycle. DFS gives a finishing order even on cyclic graphs. This order tells you which SCC should come before which — without this correct ordering, you might start from the wrong SCC and traverse in the wrong direction.

2. **Reverse all edges** in the graph.

3. **Pop from stack and run DFS** — The stack from Step 1 holds nodes in topological finish order. Pop each node: if already visited, skip it; if not visited, increment SCC count by 1 and run DFS from it on the reversed graph. All nodes reached in that DFS form one SCC.

---

## Articulation Point — Quick Intro

> An **articulation point** is a node in a graph which, if removed, breaks the graph into **two or more components**.  
> Think of it as a **single point of failure** in a system or network.

To avoid this, we use **bi-connected components** — effectively adding an **extra edge** so there are two different routes even if the articulation point is removed.
