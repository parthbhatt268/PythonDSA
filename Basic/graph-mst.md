# Spanning Tree, MST, Prim's, Kruskal's & Union-Find

---

## Spanning Tree & Minimum Spanning Tree (MST)

### Spanning Tree

- A **subset of edges** of a graph that forms a **tree** containing **all vertices** of the graph.

### Properties of a Tree

- If a tree has **N nodes**, it has **N − 1 edges**.
- A tree **cannot contain cycles**.
- **All nodes are connected**.

### Minimum Spanning Tree (MST)

- A **spanning tree** where the **sum of all edge weights is minimum**.
- A graph can have **multiple spanning trees**, but only one (or a few) with the **minimum total weight**.

**MST IS ALWAYS CALCULATED ON A CONNECTED GRAPH.**  
Graph hi connected nahi hai toh MST tree kaise connected banega.

---

## Applications of Minimum Spanning Tree (MST)

- **Railway Network Planning**  
  When building railways between multiple cities, we want **all cities (nodes) connected** while **minimizing the total construction cost** (edge weights).  
  Using a **Minimum Spanning Tree** ensures the **least overall railway cost** while keeping all cities connected.

- **Cable / Fiber Network Design**  
  When laying **internet fiber, cable TV lines, or electrical wiring** between houses or buildings, the goal is to **connect all locations while using the least total cable length**.  
  An **MST helps minimize the total wire or cable required**, reducing overall cost.

Minimum Spanning Tree last part after 01:00:00 worth watching — https://www.youtube.com/watch?v=60WK9IFnFrg&list=PLQEaRBV9gAFte7vWOl3AWFABndCRCsIRQ&index=19

---

### Finding MST Efficiently

Instead of checking all possible spanning trees, we use algorithms:

- **Prim's Algorithm**
- **Kruskal's Algorithm**

---

## Prim's Algorithm

### Steps

1. Start with **any vertex** in the graph.
2. Add the **smallest weighted edge** that connects a vertex in the tree to a vertex outside the tree.
3. Add the **new vertex** to the tree.
4. Repeat selecting the **minimum weight edge** connecting the tree to a new vertex.
5. Stop when the tree contains **all vertices (N vertices and N−1 edges)**.

### Time Complexity (TC)

- **O(E log V)** using a **priority queue / min-heap**
- **O(V²)** using a **simple adjacency matrix implementation**

### Space Complexity (SC)

- **O(V + E)** for storing the graph and auxiliary data structures.

---

## Kruskal's Algorithm

### Prim's vs Kruskal's — One Line

> **Kruskal's** works better on **sparse graphs** (fewer edges); **Prim's** works better on **dense graphs** (many edges) because Prim's picks the nearest vertex while Kruskal's sorts all edges — sorting cost dominates on dense graphs.

---

### What is Kruskal's Algorithm?

Kruskal's algorithm finds the **Minimum Spanning Tree (MST)** of a weighted, undirected graph.

A **Minimum Spanning Tree** is a subset of edges that:

- Connects **all vertices** in the graph
- Has **no cycles**
- Has the **minimum possible total edge weight**

#### Key Idea

> **Sort all edges by weight, then greedily pick the smallest edge that does NOT form a cycle.**

---

### High-Level Flow

```
1. Sort all edges in non-decreasing order of weight.
2. Initialize a Disjoint Set (Union-Find) for all vertices.
3. Iterate through sorted edges:
   a. For edge (u, v):
      - Find ultimate parent of u → pu
      - Find ultimate parent of v → pv
      - If pu ≠ pv → safe to add (no cycle) → union(u, v), add edge to MST
      - If pu == pv → skip (would form a cycle)
4. Stop when MST has (V - 1) edges.
```

---

### Why a Sorting Step? Why Not a Priority Queue?

- Kruskal's sorts **all edges upfront** — a one-time `O(E log E)` sort.
- A priority queue (min-heap) is **not needed** here because we process edges in a fixed sorted order, not dynamically.
- Prim's algorithm uses a priority queue because it grows the MST vertex by vertex and dynamically needs the next minimum-weight edge adjacent to the growing tree — that's a fundamentally different access pattern.

> **Kruskal's = sort once, sweep through. Prim's = dynamic min extraction.**

---

### Why Do We Need Disjoint Set (Union-Find)?

#### The Core Problem: Cycle Detection

When adding an edge `(u, v)`, we need to know:

> **"Are u and v already connected in our current MST?"**

If yes → adding this edge would create a **cycle** → skip it.  
If no → safe to add.

#### Why Not Just Use a `visited[]` Array?

A `visited[]` array only tells you if a node has been visited, **not whether two nodes are in the same connected component**.

Consider this scenario:

- MST so far: `1—2`, `3—4`, `5—6`
- Now you consider edge `(2, 3)`.
- Both `2` and `3` are "visited", but they are in **different components** — so the edge is safe to add.
- A visited array **cannot distinguish** between same-component and different-component.

**Disjoint Set solves this exactly**: it tracks which component each node belongs to via a **representative/ultimate parent**.

---

## Disjoint Set (Union-Find) — Deep Dive

A Disjoint Set is a data structure that maintains a collection of **non-overlapping sets** and supports two core operations efficiently:

| Operation     | Purpose                                                  |
| ------------- | -------------------------------------------------------- |
| `find(x)`     | Find the **ultimate parent (representative)** of x's set |
| `union(x, y)` | **Merge** the sets containing x and y                    |

### Internal Structure

Each node has a `parent[]` array:

- Initially: `parent[i] = i` (every node is its own parent)
- As unions happen, parents update to reflect merged sets

### What is the "Ultimate Parent"?

The **ultimate parent** (also called root or representative) is the node that is its own parent — i.e., `parent[x] == x`.

To find it, you keep following parent pointers until you reach a node that points to itself:

```
find(x):
    if parent[x] == x:
        return x
    return find(parent[x])   ← recursive climb to root
```

Two nodes `u` and `v` are in the **same component** if and only if `find(u) == find(v)`.

---

## Path Compression

Naive `find()` can be `O(N)` in the worst case (a long chain of parents). **Path compression** fixes this.

### How It Works

After finding the root, **make every node on the path point directly to the root**:

```
find(x):
    if parent[x] == x:
        return x
    parent[x] = find(parent[x])   ← flatten the path while returning
    return parent[x]
```

**Before path compression:**

```
1 → 2 → 3 → 4 (root)
```

**After `find(1)` with path compression:**

```
1 → 4
2 → 4
3 → 4
```

All future `find()` calls on these nodes are now `O(1)`.

---

## Union by Rank

When merging two sets, a naive approach always attaches one tree under the other arbitrarily. This can create unbalanced trees (long chains) → slow `find()`.

**Union by Rank** ensures the **shorter tree always gets attached under the taller tree**, keeping trees balanced.

### What is Rank?

`rank[x]` represents the **upper bound on the height** of the tree rooted at x.

```
rank[] initialized to 0 for all nodes.
```

### The Three-Step Merge Process

```
union(x, y):
    Step 1: Find ultimate parent of x → px = find(x)
    Step 2: Find ultimate parent of y → py = find(y)
    Step 3: Merge — attach smaller rank under larger rank
```

### Merge Rules (When to Increment Rank)

```
if rank[px] < rank[py]:
    parent[px] = py          ← attach px under py, rank unchanged

elif rank[px] > rank[py]:
    parent[py] = px          ← attach py under px, rank unchanged

else:  (rank[px] == rank[py])
    parent[py] = px          ← arbitrary choice: attach py under px
    rank[px] += 1            ← ⚠️ ONLY increment when ranks are equal
```

> **Key insight:** Rank only increases when two trees of equal height merge — because that's the only time the resulting tree is taller than both inputs.

### Example

```
Initially:
  1(rank 0)   2(rank 0)   3(rank 0)

union(1, 2):
  rank[1] == rank[2], so attach 2 under 1, rank[1] becomes 1
    1(rank 1)
    └── 2

union(1, 3):
  rank[1]=1 > rank[3]=0, so attach 3 under 1, rank unchanged
    1(rank 1)
    ├── 2
    └── 3
```

---

## Union by Size (Alternative to Union by Rank)

Instead of tracking tree height, you track the **number of nodes** in each set.

```
size[] initialized to 1 for all nodes.

union(x, y):
    px = find(x), py = find(y)
    if size[px] < size[py]:
        parent[px] = py
        size[py] += size[px]
    else:
        parent[py] = px
        size[px] += size[py]
```

> Always attach the **smaller set under the larger set**.  
> **Size always updates** (unlike rank which only updates on equal-rank merge).

### Rank vs Size — Which to Use?

|             | Union by Rank                  | Union by Size                 |
| ----------- | ------------------------------ | ----------------------------- |
| Tracks      | Tree height (upper bound)      | Number of nodes               |
| Update rule | Only when ranks are equal      | Always update                 |
| Both give   | Near-O(1) amortized find       | Near-O(1) amortized find      |
| Preference  | Slightly more common in theory | More intuitive, equally valid |

---

## Time Complexity of Disjoint Set Operations

With both **path compression** + **union by rank/size**:

| Operation     | Amortized Time Complexity   |
| ------------- | --------------------------- |
| `find(x)`     | O(α(N)) — inverse Ackermann |
| `union(x, y)` | O(α(N))                     |

`α(N)` (inverse Ackermann function) is essentially **constant** for all practical values of N — it is ≤ 4 for any N up to 10^(10^(10^(10^16))). So effectively **O(1) per operation**.

---

## Full Kruskal's Algorithm with Disjoint Set

```python
def kruskal(V, edges):
    # edges: list of (weight, u, v)
    edges.sort()                        # Sort by weight: O(E log E)

    parent = list(range(V))
    rank = [0] * V

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x]) # Path compression
        return parent[x]

    def union(x, y):
        px, py = find(x), find(y)
        if px == py:
            return False                # Same component → cycle
        if rank[px] < rank[py]:
            parent[px] = py
        elif rank[px] > rank[py]:
            parent[py] = px
        else:
            parent[py] = px
            rank[px] += 1
        return True

    mst_weight = 0
    mst_edges = []

    for weight, u, v in edges:
        if union(u, v):                 # No cycle → add to MST
            mst_weight += weight
            mst_edges.append((u, v, weight))
        if len(mst_edges) == V - 1:     # MST complete
            break

    return mst_weight, mst_edges
```

---

## Worked Example

**Graph:**

```
    1
  /   \
 2     3
  \   /
    4
```

Edges: `(1,2,1), (2,4,3), (1,3,2), (3,4,4), (2,3,5)`

**Step 1 — Sort edges by weight:**

```
(1,2,1), (1,3,2), (2,4,3), (3,4,4), (2,3,5)
```

**Step 2 — Process edges:**

| Edge      | find(u) | find(v) | Same?   | Action           |
| --------- | ------- | ------- | ------- | ---------------- |
| (1,2,w=1) | 1       | 2       | No      | Add → union(1,2) |
| (1,3,w=2) | 1       | 3       | No      | Add → union(1,3) |
| (2,4,w=3) | 1       | 4       | No      | Add → union(1,4) |
| (3,4,w=4) | 1       | 1       | **Yes** | Skip (cycle!)    |

**MST edges:** `(1,2), (1,3), (2,4)` → Total weight = `1 + 2 + 3 = 6`

---

## Overall Time Complexity of Kruskal's

| Step                      | Complexity         |
| ------------------------- | ------------------ |
| Sorting all edges         | O(E log E)         |
| E × union-find operations | O(E · α(V)) ≈ O(E) |
| **Total**                 | **O(E log E)**     |

For sparse graphs where `E ≈ V`, this is `O(V log V)` — very efficient.

---

## Summary of Key Concepts

| Concept                | Why It Matters                                                         |
| ---------------------- | ---------------------------------------------------------------------- |
| Sort edges by weight   | Greedy: always try cheapest edge first                                 |
| Disjoint Set           | Efficiently detects if two nodes are already connected (cycle check)   |
| Ultimate parent        | Representative of a component — if same for u and v, they're connected |
| Path compression       | Flattens trees during find → nearly O(1) per operation                 |
| Union by rank/size     | Keeps trees balanced → prevents worst-case O(N) find                   |
| Rank increment rule    | Only increment when merging two trees of equal rank                    |
| Skip if same component | Core cycle-avoidance logic                                             |
| Stop at V-1 edges      | A spanning tree on V nodes always has exactly V-1 edges                |
