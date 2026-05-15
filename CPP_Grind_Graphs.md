# 🕸️ Complete Graphs Interview Sheet — C++

> **The ultimate Graphs guide for coding interviews & tests.**
> Representation, BFS, DFS, Topo Sort, Shortest Path, MST, SCC, Bipartite, Union-Find & more.

---

## 📑 Table of Contents

**FOUNDATIONS (1–8)**
1. Graph Terminology & Types
2. Graph Representations (Adjacency Matrix, List, Edge List)
3. BFS — Iterative
4. DFS — Recursive & Iterative
5. Connected Components in Undirected Graph
6. Number of Provinces (Connected Components via DSU)
7. Cycle Detection in Undirected Graph
8. Cycle Detection in Directed Graph

**EASY — GRID PROBLEMS (9–18)**
9. Number of Islands
10. Max Area of Island
11. Surrounded Regions
12. Flood Fill
13. Rotting Oranges (Multi-source BFS)
14. 01 Matrix (Distance to Nearest 0)
15. Walls and Gates
16. Number of Closed Islands
17. Pacific Atlantic Water Flow
18. Shortest Path in Binary Matrix

**MEDIUM (19–32)**
19. Topological Sort — Kahn's Algorithm (BFS)
20. Topological Sort — DFS-based
21. Course Schedule I & II
22. Detect Cycle in Directed Graph using Topo Sort
23. Alien Dictionary
24. Find Eventual Safe States
25. Bipartite Graph Check (BFS + DFS)
26. Clone Graph
27. Word Ladder
28. Word Ladder II (All Shortest Paths)
29. Number of Distinct Islands
30. Number of Enclaves
31. Keys and Rooms
32. Minimum Genetic Mutation

**SHORTEST PATH (33–40)**
33. Dijkstra's Algorithm
34. Dijkstra with Path Reconstruction
35. Bellman-Ford Algorithm (Negative Weights)
36. Floyd-Warshall (All-pairs)
37. Shortest Path in Unweighted Graph (BFS)
38. Shortest Path in DAG (Topo + Relax)
39. Network Delay Time
40. Cheapest Flights Within K Stops

**UNION-FIND / DSU (41–46)**
41. Disjoint Set Union Implementation
42. Union by Rank & Path Compression
43. Number of Connected Components in Edge List
44. Redundant Connection
45. Accounts Merge
46. Number of Operations to Connect Network

**ADVANCED (47–60)**
47. Minimum Spanning Tree — Kruskal's
48. Minimum Spanning Tree — Prim's
49. Strongly Connected Components — Kosaraju's
50. Strongly Connected Components — Tarjan's
51. Articulation Points (Cut Vertices)
52. Bridges in Graph
53. Eulerian Path & Circuit
54. Hamiltonian Path (Backtracking)
55. Travelling Salesman (Bitmask DP)
56. Max Flow — Ford-Fulkerson / Edmonds-Karp
57. Minimum Cost Path (Dijkstra Variant)
58. A* Search Algorithm
59. 0-1 BFS (Deque)
60. Multi-source BFS Pattern

---

## ⚡ Complexity Reference

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| BFS | O(V + E) | O(V) | Unweighted shortest path, levels |
| DFS | O(V + E) | O(V) | Connectivity, cycles, topo sort |
| Dijkstra | O((V+E) log V) | O(V) | Non-negative weights |
| Bellman-Ford | O(V × E) | O(V) | Negative weights, negative cycle detect |
| Floyd-Warshall | O(V³) | O(V²) | All pairs, small V |
| Kruskal MST | O(E log E) | O(V) | Sparse graphs |
| Prim MST | O((V+E) log V) | O(V) | Dense graphs |
| Topo Sort | O(V + E) | O(V) | DAG ordering |
| Kosaraju SCC | O(V + E) | O(V) | Strongly connected |
| Tarjan SCC | O(V + E) | O(V) | One DFS pass |
| Union-Find (with optimizations) | O(α(N)) per op | O(N) | Almost constant |
| A* | O(E) typical | O(V) | Path with heuristic |

---

## 🎯 Pattern Recognition

| Problem Signal | Technique |
|----------------|-----------|
| "Shortest path, unweighted" | BFS |
| "Shortest path, non-negative weights" | Dijkstra |
| "Shortest path, negative weights" | Bellman-Ford |
| "All-pairs shortest path" | Floyd-Warshall |
| "Connected components" | DFS / BFS / Union-Find |
| "Cycle in undirected graph" | DFS (parent check) / Union-Find |
| "Cycle in directed graph" | DFS (3-color) / Kahn's |
| "Course prerequisites / dependencies" | Topological sort |
| "2-color / Can we split into 2 groups?" | Bipartite check (BFS/DFS) |
| "Minimum cost to connect all" | MST (Kruskal / Prim) |
| "Group everything connected" | Union-Find |
| "Source AND destination given, K stops" | Modified Dijkstra/BFS |
| "Grid traversal / islands" | DFS / BFS on 2D grid |
| "Multi-source spread" | Multi-source BFS (push all sources at start) |
| "Edge with weight 0 or 1 only" | 0-1 BFS with deque |
| "Path with heuristic to target" | A* search |
| "Graph with very few nodes (≤20)" | Bitmask DP (TSP) |

---

# 🟢 FOUNDATIONS

---

## 1. Graph Terminology & Types

```
Undirected:        Directed:           Weighted:
  A───B             A───>B               A──5──>B
  │   │             │                    │      │
  C───D             v                    7      2
                    C                    v      v
                                         C──3──>D
```

**Types:**
- **Undirected** — edge (u,v) goes both ways
- **Directed (Digraph)** — edge u→v one-way
- **Weighted** — edges have weights
- **DAG** (Directed Acyclic Graph) — no cycles
- **Tree** — connected, undirected, no cycles, V−1 edges
- **Bipartite** — 2-colorable (no odd cycles)
- **Complete** — every pair connected
- **Sparse** — E ≪ V²
- **Dense** — E ≈ V²

**Vocabulary:**
- **Vertex / Node**, **Edge**
- **Degree** — number of edges (in-degree, out-degree for directed)
- **Path** — sequence of vertices via edges
- **Cycle** — path that returns to start
- **Connected** — path exists between every pair
- **SCC** — Strongly Connected Component (directed)
- **MST** — Minimum Spanning Tree

---

## 2. Graph Representations

```cpp
#include <bits/stdc++.h>
using namespace std;

int V = 5;

// 1. Adjacency Matrix — O(V²) space; fast edge lookup
vector<vector<int>> matrix(V, vector<int>(V, 0));
matrix[u][v] = 1;                    // unweighted
matrix[u][v] = w;                    // weighted
// Use for dense graphs or when V is small (≤ 1000)

// 2. Adjacency List — O(V + E) space; iterate neighbors fast
vector<vector<int>> adj(V);
adj[u].push_back(v);                 // unweighted directed
adj[u].push_back(v); adj[v].push_back(u);  // undirected

// Weighted
vector<vector<pair<int,int>>> wadj(V);   // {neighbor, weight}
wadj[u].push_back({v, w});

// 3. Edge List — useful for Kruskal's, Bellman-Ford
vector<tuple<int,int,int>> edges;    // {u, v, w}
edges.push_back({u, v, w});

// Reading edges
int n, m;
cin >> n >> m;
vector<vector<int>> graph(n);
for (int i = 0; i < m; i++) {
    int u, v; cin >> u >> v;
    graph[u].push_back(v);
    graph[v].push_back(u);           // remove for directed
}
```

**Choose:**
- Adjacency list — **default for most problems** (sparse common case)
- Adjacency matrix — small V (≤ 1000), need O(1) edge query
- Edge list — Kruskal's MST, Bellman-Ford

---

## 3. BFS — Iterative

**Use cases:** shortest path in unweighted graph, level-order, connected component.

```cpp
void bfs(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);
    queue<int> q;
    q.push(start);
    visited[start] = true;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        cout << u << " ";
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                q.push(v);
            }
        }
    }
}

// BFS with distance from source
vector<int> bfsDistance(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<int> dist(n, -1);
    queue<int> q;
    q.push(start);
    dist[start] = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            if (dist[v] == -1) {
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
    return dist;
}
// TC: O(V + E) | SC: O(V)
```

> **Critical:** mark visited when you **enqueue**, not when you dequeue — otherwise duplicates pile up.

---

## 4. DFS — Recursive & Iterative

```cpp
// Recursive
void dfs(int u, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[u] = true;
    cout << u << " ";
    for (int v : adj[u]) {
        if (!visited[v]) dfs(v, adj, visited);
    }
}

// Iterative (use stack)
void dfsIter(vector<vector<int>>& adj, int start) {
    int n = adj.size();
    vector<bool> visited(n, false);
    stack<int> st;
    st.push(start);
    while (!st.empty()) {
        int u = st.top(); st.pop();
        if (visited[u]) continue;
        visited[u] = true;
        cout << u << " ";
        for (int v : adj[u]) {
            if (!visited[v]) st.push(v);
        }
    }
}
// TC: O(V + E) | SC: O(V) — recursion stack can be O(V) worst case
```

> **Stack overflow risk** for deep graphs (V ≈ 10⁵+). Use iterative DFS in those cases.

---

## 5. Connected Components

```cpp
int connectedComponents(vector<vector<int>>& adj) {
    int n = adj.size(), count = 0;
    vector<bool> visited(n, false);
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            count++;
            dfs(i, adj, visited);
        }
    }
    return count;
}
// TC: O(V + E) | SC: O(V)
```

---

## 6. Number of Provinces (LeetCode 547)

**Given `isConnected[N][N]` matrix, count groups of connected cities.**

```cpp
int findCircleNum(vector<vector<int>>& mat) {
    int n = mat.size(), count = 0;
    vector<bool> visited(n, false);
    function<void(int)> dfs = [&](int u) {
        visited[u] = true;
        for (int v = 0; v < n; v++) {
            if (mat[u][v] && !visited[v]) dfs(v);
        }
    };
    for (int i = 0; i < n; i++) {
        if (!visited[i]) { count++; dfs(i); }
    }
    return count;
}
// TC: O(N²) | SC: O(N)
// Union-Find alternative — also O(N²) with α(N) per op
```

---

## 7. Cycle Detection in Undirected Graph

```cpp
// DFS — track parent to ignore the edge we came from
bool dfsCycle(int u, int parent, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) {
            if (dfsCycle(v, u, adj, visited)) return true;
        } else if (v != parent) {
            return true;        // found visited neighbor that's not parent
        }
    }
    return false;
}

bool hasCycleUndirected(vector<vector<int>>& adj) {
    int n = adj.size();
    vector<bool> visited(n, false);
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (dfsCycle(i, -1, adj, visited)) return true;
        }
    }
    return false;
}
// TC: O(V + E) | SC: O(V)

// BFS alternative — also tracks parent
// Union-Find alternative — adding edge that connects two already-connected nodes = cycle
```

---

## 8. Cycle Detection in Directed Graph

**Use 3-color DFS** — white (unvisited), gray (in current path), black (done).

```cpp
bool dfsDirCycle(int u, vector<vector<int>>& adj, vector<int>& color) {
    color[u] = 1;           // gray (in stack)
    for (int v : adj[u]) {
        if (color[v] == 1) return true;     // back edge → cycle
        if (color[v] == 0 && dfsDirCycle(v, adj, color)) return true;
    }
    color[u] = 2;           // black (done)
    return false;
}

bool hasCycleDirected(vector<vector<int>>& adj) {
    int n = adj.size();
    vector<int> color(n, 0);
    for (int i = 0; i < n; i++) {
        if (color[i] == 0 && dfsDirCycle(i, adj, color)) return true;
    }
    return false;
}
// TC: O(V + E) | SC: O(V)

// Alternative: Kahn's algorithm — if topo sort doesn't include all nodes, there's a cycle.
```

> **Common bug:** using just a `visited` set for directed cycle detection — wrong because revisiting a black (done) node doesn't mean a cycle.

---

# 🟢 EASY — GRID PROBLEMS

---

## 9. Number of Islands (LeetCode 200)

```cpp
int numIslands(vector<vector<char>>& grid) {
    int m = grid.size(), n = grid[0].size(), count = 0;
    function<void(int,int)> dfs = [&](int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != '1') return;
        grid[r][c] = '0';                    // mark visited
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1);
    };
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == '1') { count++; dfs(r, c); }
    return count;
}
// TC: O(M × N) | SC: O(M × N) recursion worst case
```

---

## 10. Max Area of Island

```cpp
int maxAreaOfIsland(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size(), maxArea = 0;
    function<int(int,int)> dfs = [&](int r, int c) -> int {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != 1) return 0;
        grid[r][c] = 0;
        return 1 + dfs(r+1, c) + dfs(r-1, c) + dfs(r, c+1) + dfs(r, c-1);
    };
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 1) maxArea = max(maxArea, dfs(r, c));
    return maxArea;
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 11. Surrounded Regions (LeetCode 130)

**Capture all 'O' regions NOT connected to border.** Trick: mark border-connected 'O's as safe first.

```cpp
void solve(vector<vector<char>>& board) {
    int m = board.size(), n = board[0].size();
    function<void(int,int)> dfs = [&](int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || board[r][c] != 'O') return;
        board[r][c] = 'S';                   // mark safe
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1);
    };
    // Border DFS
    for (int i = 0; i < m; i++) { dfs(i, 0); dfs(i, n-1); }
    for (int j = 0; j < n; j++) { dfs(0, j); dfs(m-1, j); }
    // Final sweep
    for (int r = 0; r < m; r++) {
        for (int c = 0; c < n; c++) {
            if (board[r][c] == 'O') board[r][c] = 'X';
            else if (board[r][c] == 'S') board[r][c] = 'O';
        }
    }
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 12. Flood Fill (LeetCode 733)

```cpp
vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
    int oldColor = image[sr][sc];
    if (oldColor == newColor) return image;
    int m = image.size(), n = image[0].size();
    function<void(int,int)> dfs = [&](int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || image[r][c] != oldColor) return;
        image[r][c] = newColor;
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1);
    };
    dfs(sr, sc);
    return image;
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 13. Rotting Oranges (Multi-source BFS)

**Each rotten orange spreads to adjacent fresh oranges per minute.**

```cpp
int orangesRotting(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size(), fresh = 0, minutes = 0;
    queue<pair<int,int>> q;
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++) {
            if (grid[r][c] == 1) fresh++;
            if (grid[r][c] == 2) q.push({r, c});
        }
    int dr[] = {-1, 1, 0, 0}, dc[] = {0, 0, -1, 1};
    while (!q.empty() && fresh > 0) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            auto [r, c] = q.front(); q.pop();
            for (int d = 0; d < 4; d++) {
                int nr = r + dr[d], nc = c + dc[d];
                if (nr < 0 || nr >= m || nc < 0 || nc >= n || grid[nr][nc] != 1) continue;
                grid[nr][nc] = 2;
                fresh--;
                q.push({nr, nc});
            }
        }
        minutes++;
    }
    return fresh == 0 ? minutes : -1;
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 14. 01 Matrix — Distance to Nearest 0

**Multi-source BFS from all zeros simultaneously.**

```cpp
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int m = mat.size(), n = mat[0].size();
    vector<vector<int>> dist(m, vector<int>(n, -1));
    queue<pair<int,int>> q;
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (mat[r][c] == 0) { dist[r][c] = 0; q.push({r, c}); }
    int dr[] = {-1, 1, 0, 0}, dc[] = {0, 0, -1, 1};
    while (!q.empty()) {
        auto [r, c] = q.front(); q.pop();
        for (int d = 0; d < 4; d++) {
            int nr = r + dr[d], nc = c + dc[d];
            if (nr < 0 || nr >= m || nc < 0 || nc >= n || dist[nr][nc] != -1) continue;
            dist[nr][nc] = dist[r][c] + 1;
            q.push({nr, nc});
        }
    }
    return dist;
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 15. Walls and Gates (LeetCode 286)

**Same as 01 Matrix — multi-source BFS from gates (value 0), update empty rooms (INF).**

```cpp
void wallsAndGates(vector<vector<int>>& rooms) {
    int m = rooms.size(), n = rooms[0].size();
    queue<pair<int,int>> q;
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (rooms[r][c] == 0) q.push({r, c});
    int dr[] = {-1, 1, 0, 0}, dc[] = {0, 0, -1, 1};
    while (!q.empty()) {
        auto [r, c] = q.front(); q.pop();
        for (int d = 0; d < 4; d++) {
            int nr = r + dr[d], nc = c + dc[d];
            if (nr < 0 || nr >= m || nc < 0 || nc >= n || rooms[nr][nc] != INT_MAX) continue;
            rooms[nr][nc] = rooms[r][c] + 1;
            q.push({nr, nc});
        }
    }
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 16. Number of Closed Islands (LeetCode 1254)

**Like #11 — eliminate border-connected islands, then count remaining.**

```cpp
int closedIsland(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size(), count = 0;
    function<void(int,int)> dfs = [&](int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != 0) return;
        grid[r][c] = 1;
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1);
    };
    for (int i = 0; i < m; i++) { dfs(i, 0); dfs(i, n-1); }
    for (int j = 0; j < n; j++) { dfs(0, j); dfs(m-1, j); }
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 0) { count++; dfs(r, c); }
    return count;
}
```

---

## 17. Pacific Atlantic Water Flow (LeetCode 417)

**Cells where water flows to BOTH oceans.** Reverse-BFS from each ocean's border, find intersection.

```cpp
vector<vector<int>> pacificAtlantic(vector<vector<int>>& h) {
    int m = h.size(), n = h[0].size();
    vector<vector<bool>> pac(m, vector<bool>(n, false));
    vector<vector<bool>> atl(m, vector<bool>(n, false));
    int dr[] = {-1, 1, 0, 0}, dc[] = {0, 0, -1, 1};
    function<void(int, int, vector<vector<bool>>&)> dfs =
        [&](int r, int c, vector<vector<bool>>& ocean) {
            ocean[r][c] = true;
            for (int d = 0; d < 4; d++) {
                int nr = r + dr[d], nc = c + dc[d];
                if (nr < 0 || nr >= m || nc < 0 || nc >= n) continue;
                if (ocean[nr][nc] || h[nr][nc] < h[r][c]) continue;
                dfs(nr, nc, ocean);
            }
        };
    for (int i = 0; i < m; i++) { dfs(i, 0, pac); dfs(i, n-1, atl); }
    for (int j = 0; j < n; j++) { dfs(0, j, pac); dfs(m-1, j, atl); }
    vector<vector<int>> result;
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (pac[r][c] && atl[r][c]) result.push_back({r, c});
    return result;
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 18. Shortest Path in Binary Matrix (LeetCode 1091)

**BFS with 8 directions.** Each cell costs 1.

```cpp
int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
    int n = grid.size();
    if (grid[0][0] != 0 || grid[n-1][n-1] != 0) return -1;
    queue<tuple<int,int,int>> q;
    q.push({0, 0, 1});
    grid[0][0] = 1;
    int dr[] = {-1,-1,-1,0,0,1,1,1}, dc[] = {-1,0,1,-1,1,-1,0,1};
    while (!q.empty()) {
        auto [r, c, d] = q.front(); q.pop();
        if (r == n - 1 && c == n - 1) return d;
        for (int k = 0; k < 8; k++) {
            int nr = r + dr[k], nc = c + dc[k];
            if (nr < 0 || nr >= n || nc < 0 || nc >= n || grid[nr][nc] != 0) continue;
            grid[nr][nc] = 1;
            q.push({nr, nc, d + 1});
        }
    }
    return -1;
}
// TC: O(N²) | SC: O(N²)
```

---

# 🟡 MEDIUM PROBLEMS

---

## 19. Topological Sort — Kahn's Algorithm (BFS)

**Idea:** repeatedly remove nodes with in-degree 0.

```cpp
vector<int> topoSortKahn(vector<vector<int>>& adj) {
    int n = adj.size();
    vector<int> indeg(n, 0);
    for (int u = 0; u < n; u++)
        for (int v : adj[u]) indeg[v]++;
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    vector<int> result;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        result.push_back(u);
        for (int v : adj[u]) {
            if (--indeg[v] == 0) q.push(v);
        }
    }
    if (result.size() != n) return {};   // cycle exists
    return result;
}
// TC: O(V + E) | SC: O(V)
```

---

## 20. Topological Sort — DFS-based

```cpp
void dfsTopo(int u, vector<vector<int>>& adj, vector<bool>& visited, stack<int>& st) {
    visited[u] = true;
    for (int v : adj[u])
        if (!visited[v]) dfsTopo(v, adj, visited, st);
    st.push(u);                          // push AFTER children
}

vector<int> topoSortDFS(vector<vector<int>>& adj) {
    int n = adj.size();
    vector<bool> visited(n, false);
    stack<int> st;
    for (int i = 0; i < n; i++)
        if (!visited[i]) dfsTopo(i, adj, visited, st);
    vector<int> result;
    while (!st.empty()) { result.push_back(st.top()); st.pop(); }
    return result;
}
// TC: O(V + E) | SC: O(V)
// NOTE: DFS version doesn't detect cycles — use 3-color or Kahn's for that.
```

---

## 21. Course Schedule I & II (LeetCode 207, 210)

**I — can you finish? (cycle detection)** | **II — return order (topo sort)**

```cpp
// Course Schedule I
bool canFinish(int n, vector<vector<int>>& prereqs) {
    vector<vector<int>> adj(n);
    vector<int> indeg(n, 0);
    for (auto& p : prereqs) { adj[p[1]].push_back(p[0]); indeg[p[0]]++; }
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    int done = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        done++;
        for (int v : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    return done == n;
}

// Course Schedule II — same, but return the order
vector<int> findOrder(int n, vector<vector<int>>& prereqs) {
    vector<vector<int>> adj(n);
    vector<int> indeg(n, 0);
    for (auto& p : prereqs) { adj[p[1]].push_back(p[0]); indeg[p[0]]++; }
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    return order.size() == n ? order : vector<int>{};
}
// TC: O(V + E) | SC: O(V + E)
```

---

## 22. Detect Cycle via Topo Sort

If Kahn's algorithm processes fewer than V nodes, there's a cycle.

(See #21 — same pattern.)

---

## 23. Alien Dictionary (LeetCode 269)

**Given words sorted in alien alphabet, derive char order.**

```cpp
string alienOrder(vector<string>& words) {
    unordered_map<char, unordered_set<char>> adj;
    unordered_map<char, int> indeg;
    for (auto& w : words) for (char c : w) indeg[c] = 0;
    for (int i = 0; i + 1 < words.size(); i++) {
        string& a = words[i], b = words[i + 1];
        if (a.size() > b.size() && a.substr(0, b.size()) == b) return "";
        for (int j = 0; j < min(a.size(), b.size()); j++) {
            if (a[j] != b[j]) {
                if (!adj[a[j]].count(b[j])) {
                    adj[a[j]].insert(b[j]);
                    indeg[b[j]]++;
                }
                break;
            }
        }
    }
    queue<char> q;
    for (auto& [c, d] : indeg) if (d == 0) q.push(c);
    string result;
    while (!q.empty()) {
        char c = q.front(); q.pop();
        result += c;
        for (char nei : adj[c]) if (--indeg[nei] == 0) q.push(nei);
    }
    return result.size() == indeg.size() ? result : "";
}
// TC: O(C) where C = total chars | SC: O(1) — fixed alphabet
```

---

## 24. Find Eventual Safe States (LeetCode 802)

**A node is safe if all paths lead to terminals (no cycles).** Reverse the graph + Kahn's.

```cpp
vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<vector<int>> rev(n);
    vector<int> indeg(n, 0);
    for (int u = 0; u < n; u++)
        for (int v : graph[u]) {
            rev[v].push_back(u);
            indeg[u]++;
        }
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    vector<bool> safe(n, false);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        safe[u] = true;
        for (int v : rev[u]) if (--indeg[v] == 0) q.push(v);
    }
    vector<int> result;
    for (int i = 0; i < n; i++) if (safe[i]) result.push_back(i);
    return result;
}
// TC: O(V + E) | SC: O(V + E)
```

---

## 25. Bipartite Graph Check

**A graph is bipartite ⟺ 2-colorable ⟺ no odd cycles.**

```cpp
bool isBipartiteBFS(vector<vector<int>>& adj) {
    int n = adj.size();
    vector<int> color(n, -1);
    for (int start = 0; start < n; start++) {
        if (color[start] != -1) continue;
        queue<int> q;
        q.push(start);
        color[start] = 0;
        while (!q.empty()) {
            int u = q.front(); q.pop();
            for (int v : adj[u]) {
                if (color[v] == -1) {
                    color[v] = 1 - color[u];
                    q.push(v);
                } else if (color[v] == color[u]) {
                    return false;
                }
            }
        }
    }
    return true;
}
// TC: O(V + E) | SC: O(V)
```

---

## 26. Clone Graph (LeetCode 133)

```cpp
struct Node { int val; vector<Node*> neighbors; Node(int v) : val(v) {} };

Node* cloneGraph(Node* node) {
    if (!node) return nullptr;
    unordered_map<Node*, Node*> mp;
    function<Node*(Node*)> dfs = [&](Node* u) -> Node* {
        if (mp.count(u)) return mp[u];
        Node* copy = new Node(u->val);
        mp[u] = copy;
        for (Node* nei : u->neighbors) copy->neighbors.push_back(dfs(nei));
        return copy;
    };
    return dfs(node);
}
// TC: O(V + E) | SC: O(V)
```

---

## 27. Word Ladder (LeetCode 127)

**Shortest sequence of one-char swaps to transform start → end via valid words.**

```cpp
int ladderLength(string begin, string end, vector<string>& wordList) {
    unordered_set<string> dict(wordList.begin(), wordList.end());
    if (!dict.count(end)) return 0;
    queue<string> q; q.push(begin);
    int steps = 1;
    while (!q.empty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            string w = q.front(); q.pop();
            if (w == end) return steps;
            for (int j = 0; j < w.size(); j++) {
                char orig = w[j];
                for (char c = 'a'; c <= 'z'; c++) {
                    w[j] = c;
                    if (dict.count(w)) { q.push(w); dict.erase(w); }
                }
                w[j] = orig;
            }
        }
        steps++;
    }
    return 0;
}
// TC: O(N × L²) where N = words, L = length | SC: O(N × L)
```

---

## 28. Word Ladder II — All Shortest Paths

**BFS to find depth, then DFS backwards to reconstruct all paths.** (Complex — sketch only.)

```cpp
// BFS records parents at each level; DFS rebuilds all paths.
// Skipped for brevity — see LeetCode 126.
```

---

## 29. Number of Distinct Islands (LeetCode 694)

**Two islands same shape iff their normalized paths match.**

```cpp
int numDistinctIslands(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    unordered_set<string> shapes;
    function<void(int,int,int,int,string&)> dfs = [&](int r, int c, int br, int bc, string& path) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != 1) return;
        grid[r][c] = 0;
        path += to_string(r - br) + "," + to_string(c - bc) + ";";
        dfs(r+1, c, br, bc, path);
        dfs(r-1, c, br, bc, path);
        dfs(r, c+1, br, bc, path);
        dfs(r, c-1, br, bc, path);
    };
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 1) {
                string path;
                dfs(r, c, r, c, path);
                shapes.insert(path);
            }
    return shapes.size();
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 30. Number of Enclaves (LeetCode 1020)

**Land cells you can't walk off the boundary from.**

```cpp
int numEnclaves(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    function<void(int,int)> dfs = [&](int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != 1) return;
        grid[r][c] = 0;
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1);
    };
    for (int i = 0; i < m; i++) { dfs(i, 0); dfs(i, n-1); }
    for (int j = 0; j < n; j++) { dfs(0, j); dfs(m-1, j); }
    int count = 0;
    for (int r = 0; r < m; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 1) count++;
    return count;
}
// TC: O(M × N) | SC: O(M × N)
```

---

## 31. Keys and Rooms (LeetCode 841)

```cpp
bool canVisitAllRooms(vector<vector<int>>& rooms) {
    int n = rooms.size();
    vector<bool> visited(n, false);
    stack<int> st; st.push(0); visited[0] = true;
    int count = 1;
    while (!st.empty()) {
        int u = st.top(); st.pop();
        for (int key : rooms[u]) {
            if (!visited[key]) {
                visited[key] = true;
                count++;
                st.push(key);
            }
        }
    }
    return count == n;
}
// TC: O(V + E) | SC: O(V)
```

---

## 32. Minimum Genetic Mutation (LeetCode 433)

**Like Word Ladder, but with gene strings.** Same BFS template.

```cpp
int minMutation(string start, string end, vector<string>& bank) {
    unordered_set<string> dict(bank.begin(), bank.end());
    if (!dict.count(end)) return -1;
    queue<string> q; q.push(start);
    int steps = 0;
    while (!q.empty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            string g = q.front(); q.pop();
            if (g == end) return steps;
            for (int j = 0; j < g.size(); j++) {
                char orig = g[j];
                for (char c : {'A','C','G','T'}) {
                    g[j] = c;
                    if (dict.count(g)) { q.push(g); dict.erase(g); }
                }
                g[j] = orig;
            }
        }
        steps++;
    }
    return -1;
}
```

---

# 🟣 SHORTEST PATH ALGORITHMS

---

## 33. Dijkstra's Algorithm

**Shortest path from source, non-negative weights.**

```cpp
vector<int> dijkstra(int n, vector<vector<pair<int,int>>>& adj, int src) {
    vector<int> dist(n, INT_MAX);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    dist[src] = 0;
    pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;            // stale entry
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
// TC: O((V + E) log V) | SC: O(V)
// Doesn't work with negative edges!
```

---

## 34. Dijkstra with Path Reconstruction

```cpp
pair<vector<int>, vector<int>> dijkstraWithPath(int n, vector<vector<pair<int,int>>>& adj, int src) {
    vector<int> dist(n, INT_MAX), parent(n, -1);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    dist[src] = 0;
    pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                parent[v] = u;
                pq.push({dist[v], v});
            }
        }
    }
    return {dist, parent};
}

vector<int> reconstructPath(int src, int dst, vector<int>& parent) {
    vector<int> path;
    for (int cur = dst; cur != -1; cur = parent[cur]) path.push_back(cur);
    if (path.back() != src) return {};
    reverse(path.begin(), path.end());
    return path;
}
```

---

## 35. Bellman-Ford (Handles Negative Weights)

```cpp
vector<int> bellmanFord(int n, vector<tuple<int,int,int>>& edges, int src) {
    vector<int> dist(n, INT_MAX);
    dist[src] = 0;
    for (int i = 0; i < n - 1; i++) {           // V-1 iterations
        for (auto& [u, v, w] : edges) {
            if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }
    // Check for negative cycle
    for (auto& [u, v, w] : edges) {
        if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
            return {};       // negative cycle reachable
        }
    }
    return dist;
}
// TC: O(V × E) | SC: O(V)
```

---

## 36. Floyd-Warshall (All-Pairs Shortest Path)

```cpp
void floydWarshall(vector<vector<int>>& dist) {
    int n = dist.size();
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX
                    && dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
    // dist[i][i] < 0 ⇒ negative cycle through i
}
// TC: O(V³) | SC: O(V²)
// Good for V ≤ 500 (transitive closure, all-pairs distance)
```

---

## 37. Shortest Path in Unweighted Graph

**BFS** — distance = number of edges. (See #3.)

---

## 38. Shortest Path in DAG (Topo + Relax)

**Better than Dijkstra for DAGs — O(V + E).**

```cpp
vector<int> shortestPathDAG(int n, vector<vector<pair<int,int>>>& adj, int src) {
    vector<int> indeg(n, 0);
    for (int u = 0; u < n; u++)
        for (auto [v, w] : adj[u]) indeg[v]++;
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    vector<int> topo;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        topo.push_back(u);
        for (auto [v, w] : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    vector<int> dist(n, INT_MAX);
    dist[src] = 0;
    for (int u : topo) {
        if (dist[u] == INT_MAX) continue;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) dist[v] = dist[u] + w;
        }
    }
    return dist;
}
// TC: O(V + E) | SC: O(V)
// Works with negative edges (since it's a DAG).
```

---

## 39. Network Delay Time (LeetCode 743)

**Dijkstra from K, return max dist (or -1 if unreachable).**

```cpp
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<vector<pair<int,int>>> adj(n + 1);
    for (auto& t : times) adj[t[0]].push_back({t[1], t[2]});
    auto dist = dijkstra(n + 1, adj, k);
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        if (dist[i] == INT_MAX) return -1;
        ans = max(ans, dist[i]);
    }
    return ans;
}
```

---

## 40. Cheapest Flights Within K Stops (LeetCode 787)

**Variation of Bellman-Ford with stop limit.**

```cpp
int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
    vector<int> dist(n, INT_MAX);
    dist[src] = 0;
    for (int i = 0; i <= k; i++) {              // at most k+1 edges
        vector<int> tmp = dist;
        for (auto& f : flights) {
            int u = f[0], v = f[1], w = f[2];
            if (dist[u] != INT_MAX && dist[u] + w < tmp[v]) {
                tmp[v] = dist[u] + w;
            }
        }
        dist = tmp;
    }
    return dist[dst] == INT_MAX ? -1 : dist[dst];
}
// TC: O(K × E) | SC: O(V)
```

---

# 🟠 UNION-FIND / DSU

---

## 41. Disjoint Set Union — Basic

```cpp
class DSU {
    vector<int> parent;
public:
    DSU(int n) : parent(n) {
        iota(parent.begin(), parent.end(), 0);
    }
    int find(int x) {
        if (parent[x] == x) return x;
        return find(parent[x]);                  // O(N) worst — see #42
    }
    void unite(int x, int y) {
        parent[find(x)] = find(y);
    }
};
```

---

## 42. Union by Rank + Path Compression — OPTIMAL DSU

```cpp
class DSU {
    vector<int> parent, rank_;
public:
    DSU(int n) : parent(n), rank_(n, 0) {
        iota(parent.begin(), parent.end(), 0);
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);     // path compression
        return parent[x];
    }
    bool unite(int x, int y) {                                // union by rank
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank_[px] < rank_[py]) swap(px, py);
        parent[py] = px;
        if (rank_[px] == rank_[py]) rank_[px]++;
        return true;
    }
    bool connected(int x, int y) { return find(x) == find(y); }
};
// TC: O(α(N)) per op ≈ O(1) | SC: O(N)
```

---

## 43. Number of Connected Components in Edge List

```cpp
int countComponents(int n, vector<vector<int>>& edges) {
    DSU dsu(n);
    int components = n;
    for (auto& e : edges) {
        if (dsu.unite(e[0], e[1])) components--;
    }
    return components;
}
// TC: O(E × α(N)) | SC: O(N)
```

---

## 44. Redundant Connection (LeetCode 684)

**Find the edge whose removal makes graph a tree.**

```cpp
vector<int> findRedundantConnection(vector<vector<int>>& edges) {
    int n = edges.size();
    DSU dsu(n + 1);
    for (auto& e : edges) {
        if (!dsu.unite(e[0], e[1])) return e;
    }
    return {};
}
// TC: O(E × α(N)) | SC: O(N)
```

---

## 45. Accounts Merge (LeetCode 721)

**Merge accounts sharing any email.**

```cpp
vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
    int n = accounts.size();
    DSU dsu(n);
    unordered_map<string, int> emailToId;
    for (int i = 0; i < n; i++) {
        for (int j = 1; j < accounts[i].size(); j++) {
            string& email = accounts[i][j];
            if (emailToId.count(email)) dsu.unite(i, emailToId[email]);
            else emailToId[email] = i;
        }
    }
    unordered_map<int, set<string>> groups;
    for (auto& [email, id] : emailToId) groups[dsu.find(id)].insert(email);
    vector<vector<string>> result;
    for (auto& [id, emails] : groups) {
        vector<string> acc = {accounts[id][0]};
        acc.insert(acc.end(), emails.begin(), emails.end());
        result.push_back(acc);
    }
    return result;
}
```

---

## 46. Number of Operations to Connect Network (LeetCode 1319)

**Find redundant cables, count required additions.**

```cpp
int makeConnected(int n, vector<vector<int>>& connections) {
    if (connections.size() < n - 1) return -1;
    DSU dsu(n);
    int components = n;
    for (auto& c : connections) {
        if (dsu.unite(c[0], c[1])) components--;
    }
    return components - 1;
}
// TC: O(E × α(N)) | SC: O(N)
```

---

# 🔴 ADVANCED

---

## 47. Kruskal's MST

**Sort edges by weight, union nodes that aren't connected, skip cycles.**

```cpp
int kruskalMST(int n, vector<tuple<int,int,int>>& edges) {
    sort(edges.begin(), edges.end(),
         [](auto& a, auto& b) { return get<2>(a) < get<2>(b); });
    DSU dsu(n);
    int total = 0, count = 0;
    for (auto& [u, v, w] : edges) {
        if (dsu.unite(u, v)) {
            total += w;
            if (++count == n - 1) break;
        }
    }
    return count == n - 1 ? total : -1;        // disconnected
}
// TC: O(E log E) | SC: O(V)
```

---

## 48. Prim's MST

**Grow MST from a node — always pick minimum edge to unvisited node.**

```cpp
int primMST(int n, vector<vector<pair<int,int>>>& adj) {
    vector<bool> inMST(n, false);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, 0});
    int total = 0, count = 0;
    while (!pq.empty() && count < n) {
        auto [w, u] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        total += w;
        count++;
        for (auto [v, weight] : adj[u]) {
            if (!inMST[v]) pq.push({weight, v});
        }
    }
    return count == n ? total : -1;
}
// TC: O((V + E) log V) | SC: O(V)
```

---

## 49. Strongly Connected Components — Kosaraju's

**Two DFS passes — on original + on transposed graph.**

```cpp
void dfs1(int u, vector<vector<int>>& adj, vector<bool>& visited, stack<int>& order) {
    visited[u] = true;
    for (int v : adj[u]) if (!visited[v]) dfs1(v, adj, visited, order);
    order.push(u);
}

void dfs2(int u, vector<vector<int>>& radj, vector<bool>& visited, vector<int>& comp) {
    visited[u] = true;
    comp.push_back(u);
    for (int v : radj[u]) if (!visited[v]) dfs2(v, radj, visited, comp);
}

vector<vector<int>> kosaraju(int n, vector<vector<int>>& adj) {
    vector<bool> visited(n, false);
    stack<int> order;
    for (int i = 0; i < n; i++) if (!visited[i]) dfs1(i, adj, visited, order);

    vector<vector<int>> radj(n);
    for (int u = 0; u < n; u++)
        for (int v : adj[u]) radj[v].push_back(u);

    fill(visited.begin(), visited.end(), false);
    vector<vector<int>> sccs;
    while (!order.empty()) {
        int u = order.top(); order.pop();
        if (!visited[u]) {
            vector<int> comp;
            dfs2(u, radj, visited, comp);
            sccs.push_back(comp);
        }
    }
    return sccs;
}
// TC: O(V + E) | SC: O(V + E)
```

---

## 50. Strongly Connected Components — Tarjan's

**Single-pass DFS using low-link values.**

```cpp
class Tarjan {
    int timer = 0;
    vector<int> disc, low;
    stack<int> stk;
    vector<bool> onStack;
    vector<vector<int>> sccs;
    vector<vector<int>>* adj;

    void dfs(int u) {
        disc[u] = low[u] = timer++;
        stk.push(u); onStack[u] = true;
        for (int v : (*adj)[u]) {
            if (disc[v] == -1) {
                dfs(v);
                low[u] = min(low[u], low[v]);
            } else if (onStack[v]) {
                low[u] = min(low[u], disc[v]);
            }
        }
        if (disc[u] == low[u]) {
            vector<int> comp;
            while (true) {
                int v = stk.top(); stk.pop(); onStack[v] = false;
                comp.push_back(v);
                if (v == u) break;
            }
            sccs.push_back(comp);
        }
    }
public:
    vector<vector<int>> tarjan(int n, vector<vector<int>>& adjacency) {
        adj = &adjacency;
        disc.assign(n, -1); low.assign(n, -1); onStack.assign(n, false);
        for (int i = 0; i < n; i++) if (disc[i] == -1) dfs(i);
        return sccs;
    }
};
// TC: O(V + E) | SC: O(V)
```

---

## 51. Articulation Points (Cut Vertices)

**Vertex whose removal disconnects the graph.**

```cpp
int timer = 0;
vector<int> disc, low;
vector<bool> isArt;

void dfsArt(int u, int parent, vector<vector<int>>& adj) {
    disc[u] = low[u] = timer++;
    int children = 0;
    for (int v : adj[u]) {
        if (disc[v] == -1) {
            children++;
            dfsArt(v, u, adj);
            low[u] = min(low[u], low[v]);
            if (parent != -1 && low[v] >= disc[u]) isArt[u] = true;
        } else if (v != parent) {
            low[u] = min(low[u], disc[v]);
        }
    }
    if (parent == -1 && children > 1) isArt[u] = true;
}

vector<int> articulationPoints(int n, vector<vector<int>>& adj) {
    disc.assign(n, -1); low.assign(n, -1); isArt.assign(n, false);
    timer = 0;
    for (int i = 0; i < n; i++) if (disc[i] == -1) dfsArt(i, -1, adj);
    vector<int> result;
    for (int i = 0; i < n; i++) if (isArt[i]) result.push_back(i);
    return result;
}
// TC: O(V + E) | SC: O(V)
```

---

## 52. Bridges in Graph (LeetCode 1192)

**Edge whose removal disconnects the graph.**

```cpp
void dfsBridge(int u, int parent, vector<vector<int>>& adj,
               vector<int>& disc, vector<int>& low,
               vector<vector<int>>& bridges, int& timer) {
    disc[u] = low[u] = timer++;
    for (int v : adj[u]) {
        if (disc[v] == -1) {
            dfsBridge(v, u, adj, disc, low, bridges, timer);
            low[u] = min(low[u], low[v]);
            if (low[v] > disc[u]) bridges.push_back({u, v});
        } else if (v != parent) {
            low[u] = min(low[u], disc[v]);
        }
    }
}

vector<vector<int>> findBridges(int n, vector<vector<int>>& adj) {
    vector<int> disc(n, -1), low(n, -1);
    vector<vector<int>> bridges;
    int timer = 0;
    for (int i = 0; i < n; i++) if (disc[i] == -1) dfsBridge(i, -1, adj, disc, low, bridges, timer);
    return bridges;
}
// TC: O(V + E) | SC: O(V)
```

---

## 53. Eulerian Path / Circuit

**Eulerian path** visits every edge once. Conditions:
- **Undirected:** exactly 0 or 2 vertices with odd degree (0 = circuit, 2 = path)
- **Directed:** at most one vertex with out_deg − in_deg = 1, at most one with in_deg − out_deg = 1, rest equal; underlying graph connected

```cpp
// Hierholzer's algorithm for Euler circuit (sketch)
vector<int> eulerCircuit(int n, vector<vector<int>>& adj) {
    vector<int> circuit;
    stack<int> stk;
    stk.push(0);
    while (!stk.empty()) {
        int u = stk.top();
        if (!adj[u].empty()) {
            int v = adj[u].back();
            adj[u].pop_back();
            // For undirected, also remove (v, u)
            stk.push(v);
        } else {
            circuit.push_back(u);
            stk.pop();
        }
    }
    reverse(circuit.begin(), circuit.end());
    return circuit;
}
// TC: O(E) | SC: O(E)
```

---

## 54. Hamiltonian Path (Backtracking)

**Path that visits every vertex exactly once.** NP-Complete.

```cpp
bool hamHelper(int u, int n, int count, vector<vector<int>>& adj, vector<bool>& visited) {
    if (count == n) return true;
    for (int v : adj[u]) {
        if (!visited[v]) {
            visited[v] = true;
            if (hamHelper(v, n, count + 1, adj, visited)) return true;
            visited[v] = false;
        }
    }
    return false;
}

bool hasHamPath(int n, vector<vector<int>>& adj) {
    for (int start = 0; start < n; start++) {
        vector<bool> visited(n, false);
        visited[start] = true;
        if (hamHelper(start, n, 1, adj, visited)) return true;
    }
    return false;
}
// TC: O(N!) | SC: O(N)
```

---

## 55. Travelling Salesman (Bitmask DP)

**Visit all cities exactly once, return to start, minimize cost.** Practical for N ≤ 20.

```cpp
int tsp(int n, vector<vector<int>>& cost) {
    int FULL = (1 << n) - 1;
    vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX));
    dp[1][0] = 0;       // start at city 0
    for (int mask = 1; mask <= FULL; mask++) {
        for (int u = 0; u < n; u++) {
            if (!(mask & (1 << u)) || dp[mask][u] == INT_MAX) continue;
            for (int v = 0; v < n; v++) {
                if (mask & (1 << v)) continue;
                int newMask = mask | (1 << v);
                dp[newMask][v] = min(dp[newMask][v], dp[mask][u] + cost[u][v]);
            }
        }
    }
    int ans = INT_MAX;
    for (int u = 1; u < n; u++)
        if (dp[FULL][u] != INT_MAX)
            ans = min(ans, dp[FULL][u] + cost[u][0]);
    return ans;
}
// TC: O(N² × 2^N) | SC: O(N × 2^N)
```

---

## 56. Max Flow — Edmonds-Karp

**BFS-based Ford-Fulkerson. Find max flow from source to sink.**

```cpp
int bfsAugment(int s, int t, vector<vector<int>>& cap, vector<int>& parent) {
    int n = cap.size();
    fill(parent.begin(), parent.end(), -1);
    parent[s] = s;
    queue<pair<int,int>> q;
    q.push({s, INT_MAX});
    while (!q.empty()) {
        auto [u, flow] = q.front(); q.pop();
        for (int v = 0; v < n; v++) {
            if (parent[v] == -1 && cap[u][v] > 0) {
                parent[v] = u;
                int newFlow = min(flow, cap[u][v]);
                if (v == t) return newFlow;
                q.push({v, newFlow});
            }
        }
    }
    return 0;
}

int maxFlow(int n, int s, int t, vector<vector<int>> cap) {
    int total = 0;
    vector<int> parent(n);
    while (int aug = bfsAugment(s, t, cap, parent)) {
        total += aug;
        int v = t;
        while (v != s) {
            int u = parent[v];
            cap[u][v] -= aug;
            cap[v][u] += aug;
            v = u;
        }
    }
    return total;
}
// TC: O(V × E²) | SC: O(V²)
```

---

## 57. Minimum Cost Path (Grid, Dijkstra)

**Like Dijkstra but on grid — each cell has a cost.**

```cpp
int minCostGrid(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    priority_queue<tuple<int,int,int>, vector<tuple<int,int,int>>, greater<>> pq;
    pq.push({grid[0][0], 0, 0});
    dist[0][0] = grid[0][0];
    int dr[] = {-1,1,0,0}, dc[] = {0,0,-1,1};
    while (!pq.empty()) {
        auto [d, r, c] = pq.top(); pq.pop();
        if (r == m - 1 && c == n - 1) return d;
        if (d > dist[r][c]) continue;
        for (int k = 0; k < 4; k++) {
            int nr = r + dr[k], nc = c + dc[k];
            if (nr < 0 || nr >= m || nc < 0 || nc >= n) continue;
            int nd = d + grid[nr][nc];
            if (nd < dist[nr][nc]) {
                dist[nr][nc] = nd;
                pq.push({nd, nr, nc});
            }
        }
    }
    return -1;
}
// TC: O(M × N × log(M × N)) | SC: O(M × N)
```

---

## 58. A* Search

**Like Dijkstra but with heuristic** `f(n) = g(n) + h(n)`. Heuristic must be **admissible** (never overestimate) for optimal results.

```cpp
// Sketch — replace priority_queue ordering by f(n) instead of g(n)
// h(n) common choices: Manhattan distance, Euclidean distance, etc.
// Used heavily in pathfinding (games, robotics).
```

---

## 59. 0-1 BFS

**When edges have weights 0 or 1 — use deque, no need for Dijkstra.**

```cpp
vector<int> zeroOneBFS(int n, vector<vector<pair<int,int>>>& adj, int src) {
    vector<int> dist(n, INT_MAX);
    deque<int> dq;
    dist[src] = 0; dq.push_front(src);
    while (!dq.empty()) {
        int u = dq.front(); dq.pop_front();
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                if (w == 0) dq.push_front(v);
                else        dq.push_back(v);
            }
        }
    }
    return dist;
}
// TC: O(V + E) | SC: O(V)
// Way faster than Dijkstra when weights ∈ {0, 1}.
```

---

## 60. Multi-source BFS Pattern

**Push ALL sources at start, do regular BFS.** Used in #13, #14, #15.

```cpp
// Template:
queue<State> q;
for (auto src : sources) {
    visited[src] = true;
    q.push(src);
}
while (!q.empty()) { /* normal BFS */ }
// Distance from each cell to NEAREST source — O(V + E)
```

---

# 🎯 Bonus — Quick Reference

## ⭐ Common Templates

### DFS template

```cpp
function<void(int)> dfs = [&](int u) {
    visited[u] = true;
    // pre-order work
    for (int v : adj[u]) {
        if (!visited[v]) dfs(v);
    }
    // post-order work
};
```

### BFS template

```cpp
queue<int> q;
q.push(start);
visited[start] = true;
while (!q.empty()) {
    int u = q.front(); q.pop();
    // process u
    for (int v : adj[u]) {
        if (!visited[v]) {
            visited[v] = true;        // mark when enqueuing!
            q.push(v);
        }
    }
}
```

### Grid 4/8 directions

```cpp
int dr[] = {-1, 1, 0, 0};        // 4-directional
int dc[] = {0, 0, -1, 1};

int dr8[] = {-1,-1,-1, 0, 0, 1, 1, 1};   // 8-directional
int dc8[] = {-1, 0, 1,-1, 1,-1, 0, 1};
```

### Dijkstra template

```cpp
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
dist[src] = 0; pq.push({0, src});
while (!pq.empty()) {
    auto [d, u] = pq.top(); pq.pop();
    if (d > dist[u]) continue;        // stale
    for (auto [v, w] : adj[u]) {
        if (dist[u] + w < dist[v]) {
            dist[v] = dist[u] + w;
            pq.push({dist[v], v});
        }
    }
}
```

---

## ⭐ Top 10 Must-Know

1. **BFS** for unweighted shortest path & level traversal
2. **DFS** (recursive + iterative) for connectivity, cycles
3. **Topological sort** (Kahn + DFS) for DAGs / dependencies
4. **Cycle detection** — undirected (DFS/parent) + directed (3-color/Kahn)
5. **Dijkstra** for non-negative weighted shortest path
6. **Bellman-Ford** for negative weights / cycle detection
7. **Union-Find** with rank + path compression
8. **MST** — Kruskal & Prim
9. **Bipartite check** via 2-coloring (BFS/DFS)
10. **Multi-source BFS** for "spread from many points"

---

## ⭐ Common Interview Questions

> **Q: BFS vs DFS — when to use which?**
> A: BFS for shortest path (unweighted), level-by-level processing, multi-source spread. DFS for connectivity, cycles, topological sort, finding any path, exhaustive search.

> **Q: Why doesn't Dijkstra work with negative edges?**
> A: Dijkstra greedily commits to a node's shortest distance when it's popped. A negative edge later could reduce that distance — too late to revisit. Bellman-Ford relaxes all edges V−1 times, handling negatives.

> **Q: Time complexity of BFS / DFS?**
> A: O(V + E) for both with adjacency list. O(V²) with adjacency matrix.

> **Q: How to detect a cycle in a directed graph?**
> A: 3-color DFS (white/gray/black) — gray neighbor = back edge = cycle. Or Kahn's algorithm — if topo sort doesn't include all nodes, there's a cycle.

> **Q: How to detect a cycle in an undirected graph?**
> A: DFS while tracking parent — visited neighbor that isn't your parent = cycle. Or Union-Find: if both endpoints of an edge are in the same set, the edge creates a cycle.

> **Q: Kruskal vs Prim — which to use?**
> A: Kruskal sorts edges → Union-Find (great for sparse graphs / edge list). Prim uses priority queue from a node (great for dense graphs / adjacency list). Both produce MST.

> **Q: Time complexity of Union-Find?**
> A: O(α(N)) per op with union-by-rank + path compression, where α is the inverse Ackermann function — practically constant (≤ 4 for any N in real life).

> **Q: How does topological sort work?**
> A: For DAGs only. Kahn's: BFS using in-degree-0 queue. DFS-based: post-order push to stack, then reverse. Both O(V+E).

> **Q: What's an articulation point?**
> A: A vertex whose removal disconnects the graph. Found via DFS using discovery + low-link values (Tarjan).

> **Q: How would you check if a graph is bipartite?**
> A: 2-color it using BFS/DFS. If you can color it so no edge connects same-colored nodes, it's bipartite. Equivalent: no odd cycles.

> **Q: Adjacency list vs matrix?**
> A: List: O(V+E) space, fast neighbor iteration. Matrix: O(V²) space, O(1) edge query. Use list for sparse, matrix for dense or when you need fast edge lookups.

> **Q: When use Floyd-Warshall over multiple Dijkstras?**
> A: When you need all-pairs distances AND V is small (≤ 500). Or when negative edges exist (but no negative cycles). O(V³) vs O(V × (V + E) log V).

> **Q: What is SCC and how to find them?**
> A: SCC = max subset where every pair has bidirectional path. Kosaraju (2 passes) or Tarjan (1 pass) — both O(V+E).

> **Q: How to find shortest path in a grid with obstacles?**
> A: BFS (4 or 8 directions). Each cell is a node. If weighted (varying cell cost), use Dijkstra.

> **Q: 0-1 BFS vs Dijkstra?**
> A: 0-1 BFS — deque-based, O(V+E), only when weights are 0 or 1. Dijkstra — heap-based, O((V+E) log V), for arbitrary non-negative weights.

---

## ⭐ Common Pitfalls

✅ **Mark visited when ENQUEUING in BFS**, not when dequeuing — duplicates ruin performance.
✅ **Undirected cycle detection** needs parent tracking — don't count the edge you came from.
✅ **Directed cycle detection** — `visited` alone is insufficient; use 3 colors.
✅ **Dijkstra with negative weights** = wrong answer. Use Bellman-Ford instead.
✅ **Disconnected graphs** — loop over all nodes, run BFS/DFS for each unvisited.
✅ **Stack overflow in DFS** for large/skewed graphs — switch to iterative.
✅ **Union-Find without rank/compression** is O(N) per op — useless. Always optimize.
✅ **Recursion lambda gotcha** — `function<void(int)> dfs = [&](int u){ ... }` (not `auto`).
✅ **Self-loops** and **multi-edges** — clarify with interviewer; matter for cycle/MST.
✅ **Stale heap entries** in Dijkstra — check `if (d > dist[u]) continue;`.
✅ **Use `INT_MAX` carefully** — `INT_MAX + 1` overflows; check before adding.
✅ **Build reverse graph** for some problems (eventual safe states, etc.).
✅ **Off-by-one with grid bounds** — `r < m`, `c < n`, not `<=`.

---

## ⭐ Practice Problems

| LeetCode | Concept |
|----------|---------|
| 200, 695, 130, 733 | Grid DFS / BFS |
| 994, 542, 286 | Multi-source BFS |
| 207, 210 | Topological sort |
| 269 | Alien dictionary |
| 785, 886 | Bipartite |
| 133 | Clone graph |
| 127, 126 | Word ladder |
| 547 | Connected components |
| 261, 684 | Tree validity / Union-Find |
| 721 | Accounts merge |
| 743 | Dijkstra (Network delay) |
| 787 | Bellman-Ford / Constrained Dijkstra |
| 1631 | 0-1 BFS / Dijkstra (Path min effort) |
| 1192 | Bridges in graph |
| 1976 | Number of ways shortest path |
| 1334 | Floyd-Warshall |
| 1584 | MST (Min Cost Connect All Points) |
| 1928 | Constrained shortest path |
| 802 | Eventual safe states |
| 1129 | BFS with state (alternating colors) |
| 947 | Union-Find on stones |

---

# 💪 GO ACE THAT GRAPH QUESTION!

> **Test-day strategy:**
> 1. **Identify the graph** — what are nodes? What are edges? Directed? Weighted?
> 2. **Match the pattern** — shortest path? cycle? components? topo? MST?
> 3. **Pick representation** — adjacency list (default), edge list (Kruskal), matrix (small dense).
> 4. **State complexity** — most problems are O(V+E), shortest path is O((V+E) log V).
> 5. **Mark visited carefully** — when enqueuing in BFS, before recursing in DFS.
> 6. **Don't reinvent** — recognize standard algorithms (Dijkstra, Topo, Union-Find).
> 7. **Grid problems** — treat each cell as a node, 4 or 8 directions as edges.
>
> **You've got this! 🕸️🚀**
