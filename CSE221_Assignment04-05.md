# Graph & Grid Problems — Python Solutions

This file collects the graph/grid problems solved in this conversation, with concise explanations, algorithms, complexity, and submit-ready Python code.

---

## 1. Adjacency Matrix Representation

### Idea

For a directed weighted graph, store the weight of edge `u -> v` at:

```python
graph[u - 1][v - 1] = w
```

### Solution

```python
n, m = map(int, input().split())

graph = [[0] * n for _ in range(n)]

for _ in range(m):
    u, v, w = map(int, input().split())
    graph[u - 1][v - 1] = w

for row in graph:
    print(*row)
```

### Complexity

- Time: `O(N^2 + M)`
- Memory: `O(N^2)`

---

## 2. Adjacency List Representation

### Idea

An adjacency list stores the outgoing neighbors of each node.

```python
graph = [[] for _ in range(N + 1)]
```

### Solution

```python
n, m = map(int, input().split())

u = list(map(int, input().split()))
v = list(map(int, input().split()))
w = list(map(int, input().split()))

graph = [[] for _ in range(n + 1)]

for i in range(m):
    graph[u[i]].append((v[i], w[i]))

for i in range(1, n + 1):
    print(f"{i}:", end="")

    for vertex, weight in graph[i]:
        print(f" ({vertex},{weight})", end="")

    print()
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

## 3. Graph Metamorphosis

### Idea

Convert an adjacency-list style input into an adjacency matrix.

### Solution

```python
n = int(input())

matrix = [[0] * n for _ in range(n)]

for i in range(n):
    data = list(map(int, input().split()))
    k = data[0]

    for j in range(1, k + 1):
        neighbor = data[j]
        matrix[i][neighbor] = 1

for row in matrix:
    print(*row)
```

---

## 4. Seven Bridges / Euler Path Check

### Idea

An undirected graph has an Euler path if:

1. All vertices with non-zero degree belong to one connected component.
2. The number of odd-degree vertices is either `0` or `2`.

### Solution

```python
n, m = map(int, input().split())

u = list(map(int, input().split()))
v = list(map(int, input().split()))

degree = [0] * (n + 1)
graph = [[] for _ in range(n + 1)]

for i in range(m):
    a = u[i]
    b = v[i]

    degree[a] += 1
    degree[b] += 1

    graph[a].append(b)
    graph[b].append(a)

start = -1

for i in range(1, n + 1):
    if degree[i] > 0:
        start = i
        break

if start != -1:
    visited = [False] * (n + 1)
    stack = [start]
    visited[start] = True

    while stack:
        node = stack.pop()

        for neighbor in graph[node]:
            if not visited[neighbor]:
                visited[neighbor] = True
                stack.append(neighbor)

    for i in range(1, n + 1):
        if degree[i] > 0 and not visited[i]:
            print("NO")
            exit()

odd = 0

for i in range(1, n + 1):
    if degree[i] % 2 == 1:
        odd += 1

if odd == 0 or odd == 2:
    print("YES")
else:
    print("NO")
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

## 5. Edge Queries — Indegree Minus Outdegree

### Idea

For each directed edge `u -> v`:

- `u` loses one because of one outgoing edge.
- `v` gains one because of one incoming edge.

So we can directly calculate:

```text
indegree - outdegree
```

### Solution

```python
n, m = map(int, input().split())

u = list(map(int, input().split()))
v = list(map(int, input().split()))

answer = [0] * (n + 1)

for i in range(m):
    answer[u[i]] -= 1
    answer[v[i]] += 1

print(*answer[1:])
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N)`

---

## 6. King Moves on a Chessboard

### Idea

A king can move to any of the 8 surrounding cells, as long as the new position is inside the board.

### Solution

```python
n = int(input())

x, y = map(int, input().split())

moves = []

for dx in range(-1, 2):
    for dy in range(-1, 2):

        if dx == 0 and dy == 0:
            continue

        nx = x + dx
        ny = y + dy

        if 1 <= nx <= n and 1 <= ny <= n:
            moves.append((nx, ny))

print(len(moves))

for x, y in moves:
    print(x, y)
```

---

## 7. Knight Attack Detection

### Idea

A knight has exactly 8 possible relative moves.

Store all knight positions in a `set`, then for every knight check whether another knight exists at one of its attack positions.

### Solution

```python
n, m, k = map(int, input().split())

knights = set()

for _ in range(k):
    x, y = map(int, input().split())
    knights.add((x, y))

moves = [
    (-2, -1),
    (-2, 1),
    (-1, -2),
    (-1, 2),
    (1, -2),
    (1, 2),
    (2, -1),
    (2, 1)
]

for x, y in knights:
    for dx, dy in moves:
        nx = x + dx
        ny = y + dy

        if (nx, ny) in knights:
            print("YES")
            exit()

print("NO")
```

### Complexity

- Time: `O(K)`
- Memory: `O(K)`

---

## 8. Coprime Graph

### Idea

Two different nodes `x` and `y` are connected when:

```text
gcd(x, y) = 1
```

For each query `(x, k)`, return the `k`-th smallest neighbor of `x`.

### Solution

```python
from math import gcd

n, q = map(int, input().split())

graph = [[] for _ in range(n + 1)]

for i in range(1, n + 1):
    for j in range(1, n + 1):
        if i != j and gcd(i, j) == 1:
            graph[i].append(j)

for _ in range(q):
    x, k = map(int, input().split())

    if k <= len(graph[x]):
        print(graph[x][k - 1])
    else:
        print(-1)
```

---

# BFS / DFS Problems

---

## 9. BFS Traversal from Node 1

### Idea

For an undirected unweighted graph:

1. Build an adjacency list.
2. Start BFS from node `1`.
3. Use a queue.
4. Mark nodes visited when they are added to the queue.

### Solution

```python
import sys
from collections import deque

input = sys.stdin.readline

N, M = map(int, input().split())

graph = [[] for _ in range(N + 1)]

for _ in range(M):
    u, v = map(int, input().split())
    graph[u].append(v)
    graph[v].append(u)

visited = [False] * (N + 1)

queue = deque([1])
visited[1] = True

order = []

while queue:
    node = queue.popleft()
    order.append(node)

    for neighbor in graph[node]:
        if not visited[neighbor]:
            visited[neighbor] = True
            queue.append(neighbor)

print(*order)
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

## 10. DFS Traversal from Node 1

### Idea

Because `N` can be large, iterative DFS avoids Python recursion-depth problems.

This version simulates recursive DFS closely by storing:

```python
(node, next_neighbor_index)
```

### Solution

```python
import sys

input = sys.stdin.readline

N, M = map(int, input().split())

u = list(map(int, input().split()))
v = list(map(int, input().split()))

graph = [[] for _ in range(N + 1)]

for i in range(M):
    graph[u[i]].append(v[i])
    graph[v[i]].append(u[i])

visited = [False] * (N + 1)

visited[1] = True
order = [1]

stack = [(1, 0)]

while stack:
    node, idx = stack[-1]

    if idx == len(graph[node]):
        stack.pop()
        continue

    neighbor = graph[node][idx]
    stack[-1] = (node, idx + 1)

    if not visited[neighbor]:
        visited[neighbor] = True
        order.append(neighbor)
        stack.append((neighbor, 0))

print(*order)
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

## 11. Lightning McQueen — Lexicographically Smallest Shortest Path

### Idea

We need:

1. A shortest path from `S` to `D`.
2. Among all shortest paths, the lexicographically smallest one.

Run BFS from `D` to find every node's distance to `D`.

Then starting at `S`, repeatedly choose the smallest neighbor that satisfies:

```python
dist[neighbor] == dist[current] - 1
```

### Solution

```python
import sys
from collections import deque

input = sys.stdin.readline

N, M, S, D = map(int, input().split())

u = list(map(int, input().split()))
v = list(map(int, input().split()))

graph = [[] for _ in range(N + 1)]

for i in range(M):
    graph[u[i]].append(v[i])
    graph[v[i]].append(u[i])

dist = [-1] * (N + 1)

queue = deque([D])
dist[D] = 0

while queue:
    node = queue.popleft()

    for neighbor in graph[node]:
        if dist[neighbor] == -1:
            dist[neighbor] = dist[node] + 1
            queue.append(neighbor)

if dist[S] == -1:
    print(-1)
    sys.exit()

path = [S]
current = S

while current != D:
    next_node = N + 1

    for neighbor in graph[current]:
        if dist[neighbor] == dist[current] - 1:
            if neighbor < next_node:
                next_node = neighbor

    current = next_node
    path.append(current)

print(dist[S])
print(*path)
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

## 12. Through the Jungle — Shortest Path Through K

### Idea

The graph is directed and the required path must pass through `K`.

Therefore:

```text
S -> K -> D
```

Run BFS twice:

1. `S -> K`
2. `K -> D`

Then combine the paths.

### Solution

```python
import sys
from collections import deque

input = sys.stdin.readline

N, M, S, D, K = map(int, input().split())

graph = [[] for _ in range(N + 1)]

for _ in range(M):
    u, v = map(int, input().split())
    graph[u].append(v)


def bfs(start, target):
    parent = [-1] * (N + 1)
    visited = [False] * (N + 1)

    queue = deque([start])
    visited[start] = True

    while queue:
        node = queue.popleft()

        if node == target:
            break

        for neighbor in graph[node]:
            if not visited[neighbor]:
                visited[neighbor] = True
                parent[neighbor] = node
                queue.append(neighbor)

    if not visited[target]:
        return None

    path = []
    current = target

    while current != -1:
        path.append(current)

        if current == start:
            break

        current = parent[current]

    path.reverse()

    return path


path1 = bfs(S, K)
path2 = bfs(K, D)

if path1 is None or path2 is None:
    print(-1)
else:
    full_path = path1 + path2[1:]

    print(len(full_path) - 1)
    print(*full_path)
```

### Complexity

Two BFS traversals still give:

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

# Tree Problems

---

## 13. Subtree Size Queries

### Problem

A tree has `N` nodes and is rooted at `R`.

For each query `X`, find the number of nodes in the subtree rooted at `X`.

### Idea

For every node:

```text
subtree[node] = 1 + sum(subtree[child])
```

Use iterative DFS to determine the parent of each node and get a traversal order.

Then process nodes in reverse order so children contribute to parents.

### Solution

```python
import sys

input = sys.stdin.readline

N, R = map(int, input().split())

graph = [[] for _ in range(N + 1)]

for _ in range(N - 1):
    u, v = map(int, input().split())
    graph[u].append(v)
    graph[v].append(u)

parent = [0] * (N + 1)
parent[R] = -1

order = []
stack = [R]

while stack:
    node = stack.pop()
    order.append(node)

    for neighbor in graph[node]:
        if neighbor != parent[node]:
            parent[neighbor] = node
            stack.append(neighbor)

subtree = [1] * (N + 1)

for node in reversed(order):
    if parent[node] != -1:
        subtree[parent[node]] += subtree[node]

Q = int(input())

for _ in range(Q):
    X = int(input())
    print(subtree[X])
```

### Complexity

- Preprocessing: `O(N)`
- Each query: `O(1)`
- Total: `O(N + Q)`

---

# Connectivity Problems

---

## 14. Reachability Queries in an Undirected Graph

### Problem

For every query `(x, y)`, determine whether city `y` can be reached from city `x`.

### Idea

Two nodes are reachable from one another exactly when they belong to the same connected component.

A clean solution is **Disjoint Set Union (DSU / Union-Find)**.

### Solution

```python
import sys

input = sys.stdin.readline

N, M, Q = map(int, input().split())

parent = list(range(N + 1))
size = [1] * (N + 1)


def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x


def union(a, b):
    a = find(a)
    b = find(b)

    if a == b:
        return

    if size[a] < size[b]:
        a, b = b, a

    parent[b] = a
    size[a] += size[b]


for _ in range(M):
    u, v = map(int, input().split())
    union(u, v)

for _ in range(Q):
    x, y = map(int, input().split())

    if find(x) == find(y):
        print("YES")
    else:
        print("NO")
```

### Complexity

With path compression and union by size:

- Approximately `O(M + Q)`
- More precisely: `O((M + Q) * alpha(N))`

---

# Directed Graph Problems

---

## 15. Cycle Detection in a Directed Graph

### Idea

Use **Kahn's Algorithm** for topological sorting.

If the graph is acyclic, every node can eventually be removed.

If fewer than `N` nodes are processed, a cycle exists.

### Solution

```python
import sys
from collections import deque

input = sys.stdin.readline

N, M = map(int, input().split())

graph = [[] for _ in range(N + 1)]
indegree = [0] * (N + 1)

for _ in range(M):
    u, v = map(int, input().split())

    graph[u].append(v)
    indegree[v] += 1

queue = deque()

for node in range(1, N + 1):
    if indegree[node] == 0:
        queue.append(node)

count = 0

while queue:
    node = queue.popleft()
    count += 1

    for neighbor in graph[node]:
        indegree[neighbor] -= 1

        if indegree[neighbor] == 0:
            queue.append(neighbor)

if count == N:
    print("NO")
else:
    print("YES")
```

### Complexity

- Time: `O(N + M)`
- Memory: `O(N + M)`

---

# Grid Problems

---

## 16. Maximum Diamonds in a Grid

### Problem

Each grid cell is:

- `.` — free cell
- `D` — diamond
- `#` — obstacle

You may start from any non-obstacle cell and move up, down, left, or right.

Find the maximum number of diamonds collectable from one starting cell.

### Idea

All reachable non-obstacle cells form a connected component.

For each component:

1. Traverse it with DFS/BFS.
2. Count how many `D` cells it contains.
3. Keep the maximum count.

### Solution

```python
import sys

input = sys.stdin.readline

R, H = map(int, input().split())

grid = [input().strip() for _ in range(R)]

visited = [bytearray(H) for _ in range(R)]

directions = [
    (-1, 0),
    (1, 0),
    (0, -1),
    (0, 1)
]

answer = 0

for r in range(R):
    for c in range(H):

        if grid[r][c] == '#' or visited[r][c]:
            continue

        stack = [(r, c)]
        visited[r][c] = 1

        diamonds = 0

        while stack:
            x, y = stack.pop()

            if grid[x][y] == 'D':
                diamonds += 1

            for dx, dy in directions:
                nx = x + dx
                ny = y + dy

                if (
                    0 <= nx < R
                    and 0 <= ny < H
                    and not visited[nx][ny]
                    and grid[nx][ny] != '#'
                ):
                    visited[nx][ny] = 1
                    stack.append((nx, ny))

        answer = max(answer, diamonds)

print(answer)
```

### Complexity

- Time: `O(R * H)`
- Memory: `O(R * H)`

---

# Python Patterns Used in These Problems

---

## Reading a Character Grid

```python
grid = [input().strip() for _ in range(R)]
```

Equivalent to:

```python
grid = []

for _ in range(R):
    row = input().strip()
    grid.append(row)
```

Example input:

```text
.#D
..#
D..
```

becomes:

```python
grid = [
    ".#D",
    "..#",
    "D.."
]
```

You can access a cell using:

```python
grid[row][column]
```

---

## Reading an Integer Matrix

```python
matrix = [list(map(int, input().split())) for _ in range(R)]
```

For input:

```text
1 2 3
4 5 6
7 8 9
```

this creates:

```python
[
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

---

## Creating an Empty R x C Matrix

```python
matrix = [[0] * C for _ in range(R)]
```

Do **not** use:

```python
matrix = [[0] * C] * R
```

because the rows will reference the same inner list.

---

## Creating an Adjacency List

```python
graph = [[] for _ in range(N + 1)]
```

For `N = 4`:

```python
graph = [
    [],
    [],
    [],
    [],
    []
]
```

Index `0` is unused so node numbers can directly match list indices.

For an undirected edge:

```python
u, v = map(int, input().split())

graph[u].append(v)
graph[v].append(u)
```

For a directed edge:

```python
graph[u].append(v)
```

---

## BFS Template

```python
from collections import deque

queue = deque([start])
visited[start] = True

while queue:
    node = queue.popleft()

    for neighbor in graph[node]:
        if not visited[neighbor]:
            visited[neighbor] = True
            queue.append(neighbor)
```

Use BFS when you need:

- Shortest paths in unweighted graphs
- Level-by-level traversal
- Reachability
- Connected components

---

## Iterative DFS Template

```python
stack = [start]
visited[start] = True

while stack:
    node = stack.pop()

    for neighbor in graph[node]:
        if not visited[neighbor]:
            visited[neighbor] = True
            stack.append(neighbor)
```

Use DFS when you need:

- Graph traversal
- Connected components
- Tree processing
- Flood fill / grid traversal

---

## Undirected vs Directed Graph

### Undirected

For:

```text
u -- v
```

store both directions:

```python
graph[u].append(v)
graph[v].append(u)
```

### Directed

For:

```text
u -> v
```

store only:

```python
graph[u].append(v)
```

---

# Quick Problem-to-Algorithm Guide

| Problem | Main Algorithm |
|---|---|
| Adjacency Matrix | Graph representation |
| Adjacency List | Graph representation |
| Graph Metamorphosis | List -> Matrix |
| Seven Bridges | Euler path + degree |
| Edge Queries | Indegree / outdegree |
| King | Grid movement |
| Knights | Set + movement offsets |
| Coprime Graph | GCD graph |
| BFS Traversal | BFS |
| DFS Traversal | DFS |
| Lightning McQueen | BFS shortest path |
| Through the Jungle | Two BFS traversals |
| Subtree Queries | DFS + DP on tree |
| Reachability Queries | DSU / Union-Find |
| Directed Cycle | Kahn's algorithm |
| Maximum Diamonds | Connected components / flood fill |

---

# Complexity Cheat Sheet

| Algorithm | Typical Complexity |
|---|---|
| BFS | `O(N + M)` |
| DFS | `O(N + M)` |
| DSU | Almost `O(1)` per operation |
| Kahn's Topological Sort | `O(N + M)` |
| Tree subtree preprocessing | `O(N)` |
| Grid traversal | `O(R * C)` |
| Adjacency Matrix memory | `O(N^2)` |
| Adjacency List memory | `O(N + M)` |
