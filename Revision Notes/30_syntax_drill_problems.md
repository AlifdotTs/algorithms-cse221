# 30 C++ Syntax Drill Problems

These problems are designed for syntax comfort, not algorithmic cleverness.

Goal:

> Get used to arrays, vectors, structs, linked lists, trees, heaps, queues, stacks, maps, sets, graphs, grids, and recursion-related syntax.

Important rule:

- These are not meant to test advanced optimization.
- Most can be solved directly.
- Each problem includes explanation and snippets.
- Full solution is intentionally not given.

---

# How to Use This Set

For each problem:

1. Read the task.
2. Write the C++ template from memory.
3. Declare the needed data structure.
4. Write the input code.
5. Write the core logic.
6. Compile and fix syntax errors.
7. Only then compare with the snippets.

The point is not speed at first.  
The point is making syntax boring.

---

# Problem 1 — Read and Print an Array

**Difficulty:** Easy  
**Focus:** `vector<int>`, input loop, output loop

## Task

Given `N` integers, read them into a vector and print them in the same order.

## Syntax you practice

```cpp
vector<int> a(n);
```

## Core snippet

```cpp
int n;
cin >> n;

vector<int> a(n);

for (int i = 0; i < n; i++) {
    cin >> a[i];
}

for (int i = 0; i < n; i++) {
    cout << a[i] << " ";
}
cout << '\n';
```

## Explanation

This is the base pattern for almost every array problem.

---

# Problem 2 — Reverse Print an Array

**Difficulty:** Easy  
**Focus:** reverse loop

## Task

Read `N` integers and print them from last to first.

## Core snippet

```cpp
for (int i = n - 1; i >= 0; i--) {
    cout << a[i] << " ";
}
cout << '\n';
```

## Watch out

Use `int i`, not unsigned type, because `i >= 0` with unsigned types can behave badly.

---

# Problem 3 — Find Minimum and Maximum

**Difficulty:** Easy  
**Focus:** variables, loop comparison

## Task

Read `N` integers and print the minimum and maximum.

## Core snippet

```cpp
int mn = a[0];
int mx = a[0];

for (int i = 1; i < n; i++) {
    if (a[i] < mn) {
        mn = a[i];
    }

    if (a[i] > mx) {
        mx = a[i];
    }
}
```

## Explanation

Initialize with the first element. Then compare every other element.

---

# Problem 4 — Sum Using `long long`

**Difficulty:** Easy  
**Focus:** overflow prevention

## Task

Read `N` integers and print their sum.

## Core snippet

```cpp
long long sum = 0;

for (int i = 0; i < n; i++) {
    sum += a[i];
}
```

## Why `long long`?

Even if each number fits in `int`, the total sum may not.

---

# Problem 5 — Count Even and Odd

**Difficulty:** Easy  
**Focus:** conditionals, modulo

## Task

Read an array and count how many numbers are even and how many are odd.

## Core snippet

```cpp
int even = 0;
int odd = 0;

for (int x : a) {
    if (x % 2 == 0) {
        even++;
    }
    else {
        odd++;
    }
}
```

---

# Problem 6 — Manual Basic Arrangement

**Difficulty:** Easy-Medium  
**Focus:** nested loops, manual swap

## Task

Read `N` integers and arrange them in non-decreasing order without using built-in ordering functions.

## Core snippet

```cpp
for (int i = 0; i < n; i++) {
    int best = i;

    for (int j = i + 1; j < n; j++) {
        if (a[j] < a[best]) {
            best = j;
        }
    }

    int temp = a[i];
    a[i] = a[best];
    a[best] = temp;
}
```

## Explanation

At each index, find the smallest remaining value and place it there.

---

# Problem 7 — Student Record Printing

**Difficulty:** Easy-Medium  
**Focus:** `struct`, `vector<struct>`

## Task

Given student IDs and marks, store them in a struct and print them.

## Struct snippet

```cpp
struct Student {
    int id;
    int mark;
};
```

## Core snippet

```cpp
vector<Student> students(n);

for (int i = 0; i < n; i++) {
    cin >> students[i].id;
}

for (int i = 0; i < n; i++) {
    cin >> students[i].mark;
}
```

## Print snippet

```cpp
for (int i = 0; i < n; i++) {
    cout << "ID: " << students[i].id
         << " Mark: " << students[i].mark << '\n';
}
```

---

# Problem 8 — Student Ranking Manually

**Difficulty:** Medium  
**Focus:** struct comparison, manual arrangement

## Task

Rank students by:

1. higher mark first
2. if mark ties, lower ID first

## Comparison snippet

```cpp
bool better(Student a, Student b) {
    if (a.mark != b.mark) {
        return a.mark > b.mark;
    }

    return a.id < b.id;
}
```

## Arrangement snippet

```cpp
for (int i = 0; i < n; i++) {
    int best = i;

    for (int j = i + 1; j < n; j++) {
        if (better(students[j], students[best])) {
            best = j;
        }
    }

    Student temp = students[i];
    students[i] = students[best];
    students[best] = temp;
}
```

---

# Problem 9 — Matrix Input and Print

**Difficulty:** Easy  
**Focus:** `vector<vector<int>>`, nested loops

## Task

Read an `n x m` matrix and print it.

## Core snippet

```cpp
vector<vector<int>> mat(n, vector<int>(m));

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        cin >> mat[i][j];
    }
}
```

## Output snippet

```cpp
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        cout << mat[i][j] << " ";
    }
    cout << '\n';
}
```

---

# Problem 10 — Row Sum and Column Sum

**Difficulty:** Easy-Medium  
**Focus:** 2D vector indexing

## Task

Given an `n x m` matrix, print the sum of each row and each column.

## Row sum snippet

```cpp
for (int i = 0; i < n; i++) {
    int rowSum = 0;

    for (int j = 0; j < m; j++) {
        rowSum += mat[i][j];
    }

    cout << rowSum << '\n';
}
```

## Column sum snippet

```cpp
for (int j = 0; j < m; j++) {
    int colSum = 0;

    for (int i = 0; i < n; i++) {
        colSum += mat[i][j];
    }

    cout << colSum << '\n';
}
```

---

# Problem 11 — Transpose Matrix

**Difficulty:** Medium  
**Focus:** new 2D vector dimensions

## Task

Given an `n x m` matrix, print its transpose.

## Snippet

```cpp
vector<vector<int>> trans(m, vector<int>(n));

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        trans[j][i] = mat[i][j];
    }
}
```

## Explanation

Original index:

```text
mat[i][j]
```

becomes:

```text
trans[j][i]
```

---

# Problem 12 — Frequency Count With Map

**Difficulty:** Easy-Medium  
**Focus:** `map<int,int>`

## Task

Given `N` numbers, print each unique number and how many times it appears.

## Snippet

```cpp
map<int, int> freq;

for (int x : a) {
    freq[x]++;
}
```

Loop:

```cpp
for (pair<int, int> p : freq) {
    cout << p.first << " " << p.second << '\n';
}
```

---

# Problem 13 — Unique Values With Set

**Difficulty:** Easy  
**Focus:** `set<int>`

## Task

Given `N` numbers, print all unique values.

## Snippet

```cpp
set<int> values;

for (int x : a) {
    values.insert(x);
}

for (int x : values) {
    cout << x << " ";
}
```

---

# Problem 14 — Balanced Parentheses

**Difficulty:** Medium  
**Focus:** `stack<char>`

## Task

Given a string containing `(` and `)`, check if it is balanced.

## Snippet

```cpp
stack<char> st;

for (char c : s) {
    if (c == '(') {
        st.push(c);
    }
    else if (c == ')') {
        if (st.empty()) {
            // invalid
        }
        else {
            st.pop();
        }
    }
}
```

At the end:

```cpp
if (st.empty()) {
    cout << "YES\n";
}
else {
    cout << "NO\n";
}
```

---

# Problem 15 — Browser Back Button Simulation

**Difficulty:** Medium  
**Focus:** stack

## Task

You are given page visits and back commands. Use a stack to simulate the current page.

Example commands:

```text
VISIT google
VISIT github
BACK
```

## Snippet

```cpp
stack<string> history;

history.push(page);

if (!history.empty()) {
    history.pop();
}
```

## Explanation

The last visited page is the first one removed when going back.

---

# Problem 16 — Queue Line Simulation

**Difficulty:** Easy-Medium  
**Focus:** `queue<string>`

## Task

Simulate people joining and leaving a line.

Commands:

```text
JOIN name
SERVE
FRONT
```

## Snippet

```cpp
queue<string> q;

q.push(name);

if (!q.empty()) {
    cout << q.front() << '\n';
    q.pop();
}
```

---

# Problem 17 — Simple Task Priority

**Difficulty:** Medium  
**Focus:** `priority_queue<int>`

## Task

Given task priorities, always process the highest priority first.

## Snippet

```cpp
priority_queue<int> pq;

pq.push(priority);

int highest = pq.top();
pq.pop();
```

## Explanation

Default priority queue gives the largest value first.

---

# Problem 18 — Min-Heap Practice

**Difficulty:** Medium  
**Focus:** min-heap syntax

## Task

Given numbers, repeatedly print and remove the smallest number.

## Snippet

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

Use:

```cpp
pq.push(x);
cout << pq.top() << '\n';
pq.pop();
```

---

# Problem 19 — Build and Print Linked List

**Difficulty:** Medium  
**Focus:** `struct Node`, pointers, `->`

## Task

Read `N` numbers, insert them at the tail of a linked list, then print the list.

## Node snippet

```cpp
struct Node {
    int value;
    Node* next;
};
```

## Create node snippet

```cpp
Node* newNode = new Node;
newNode->value = x;
newNode->next = nullptr;
```

## Traverse snippet

```cpp
Node* current = head;

while (current != nullptr) {
    cout << current->value << " ";
    current = current->next;
}
```

---

# Problem 20 — Insert at Head in Linked List

**Difficulty:** Medium  
**Focus:** pointer rewiring

## Task

Read `N` numbers and insert each at the head of the linked list. Print final list.

## Snippet

```cpp
Node* newNode = new Node;
newNode->value = x;
newNode->next = head;
head = newNode;
```

## Explanation

The newest node becomes the first node.

---

# Problem 21 — Search in Linked List

**Difficulty:** Medium  
**Focus:** while loop with pointer

## Task

Given a linked list and target `X`, print whether `X` exists.

## Snippet

```cpp
Node* current = head;
bool found = false;

while (current != nullptr) {
    if (current->value == x) {
        found = true;
        break;
    }

    current = current->next;
}
```

---

# Problem 22 — Create a Simple Binary Tree Manually

**Difficulty:** Medium  
**Focus:** tree node syntax

## Task

Create this tree manually and print its root, left child, and right child.

```text
      10
     /  \
    5    15
```

## Node snippet

```cpp
struct Node {
    int value;
    Node* left;
    Node* right;
};
```

## Helper snippet

```cpp
Node* createNode(int value) {
    Node* node = new Node;

    node->value = value;
    node->left = nullptr;
    node->right = nullptr;

    return node;
}
```

## Build snippet

```cpp
Node* root = createNode(10);
root->left = createNode(5);
root->right = createNode(15);
```

---

# Problem 23 — Tree Traversal Printing

**Difficulty:** Medium  
**Focus:** tree recursion

## Task

Given a binary tree created manually, print preorder, inorder, and postorder traversals.

## Preorder snippet

```cpp
void preorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    cout << root->value << " ";
    preorder(root->left);
    preorder(root->right);
}
```

## Explanation

Tree traversal is recursion because every child is itself a smaller tree.

---

# Problem 24 — Count Nodes in a Binary Tree

**Difficulty:** Medium  
**Focus:** recursive return values

## Task

Count how many nodes are in a binary tree.

## Snippet

```cpp
int countNodes(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    return 1 + countNodes(root->left) + countNodes(root->right);
}
```

## Real-life analogy

Count this manager plus everyone in the left department plus everyone in the right department.

---

# Problem 25 — Height of a Binary Tree

**Difficulty:** Medium  
**Focus:** recursive max idea

## Task

Find the height of a binary tree.

## Snippet

```cpp
int height(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    int leftHeight = height(root->left);
    int rightHeight = height(root->right);

    if (leftHeight > rightHeight) {
        return 1 + leftHeight;
    }

    return 1 + rightHeight;
}
```

---

# Problem 26 — Graph Degree Counter

**Difficulty:** Easy-Medium  
**Focus:** adjacency list

## Task

Given an undirected graph, print the degree of every node.

## Snippet

```cpp
vector<vector<int>> adj(n + 1);

for (int i = 0; i < m; i++) {
    int u, v;
    cin >> u >> v;

    adj[u].push_back(v);
    adj[v].push_back(u);
}
```

Degree:

```cpp
cout << adj[i].size() << '\n';
```

---

# Problem 27 — Print Graph Adjacency List

**Difficulty:** Easy-Medium  
**Focus:** nested vector loops

## Task

Given a graph, print each node's neighbors.

## Snippet

```cpp
for (int u = 1; u <= n; u++) {
    cout << u << ": ";

    for (int v : adj[u]) {
        cout << v << " ";
    }

    cout << '\n';
}
```

---

# Problem 28 — BFS Visit Order

**Difficulty:** Medium  
**Focus:** queue, graph traversal

## Task

Given an undirected graph and source node, print nodes in BFS order.

## Snippet

```cpp
vector<int> visited(n + 1, 0);
queue<int> q;

visited[source] = 1;
q.push(source);

while (!q.empty()) {
    int u = q.front();
    q.pop();

    cout << u << " ";

    for (int v : adj[u]) {
        if (!visited[v]) {
            visited[v] = 1;
            q.push(v);
        }
    }
}
```

---

# Problem 29 — Count Connected Components

**Difficulty:** Medium  
**Focus:** graph BFS/DFS over disconnected graph

## Task

Given an undirected graph, count how many connected components it has.

## Snippet

```cpp
int components = 0;

for (int i = 1; i <= n; i++) {
    if (!visited[i]) {
        components++;

        // run BFS or DFS from i
    }
}
```

## BFS start snippet

```cpp
visited[i] = 1;
q.push(i);
```

---

# Problem 30 — Grid Flood Fill

**Difficulty:** Medium  
**Focus:** `vector<string>`, 2D visited, queue of pairs

## Task

Given a grid of `.` and `#`, starting from a cell, mark all reachable `.` cells.

## Direction arrays

```cpp
int dx[] = {-1, 1, 0, 0};
int dy[] = {0, 0, -1, 1};
```

## Valid function

```cpp
bool valid(int x, int y, int n, int m) {
    return x >= 0 && x < n && y >= 0 && y < m;
}
```

## BFS snippet

```cpp
queue<pair<int, int>> q;
vector<vector<int>> visited(n, vector<int>(m, 0));

visited[sx][sy] = 1;
q.push({sx, sy});

while (!q.empty()) {
    pair<int, int> cell = q.front();
    q.pop();

    int x = cell.first;
    int y = cell.second;

    for (int k = 0; k < 4; k++) {
        int nx = x + dx[k];
        int ny = y + dy[k];

        if (valid(nx, ny, n, m) && !visited[nx][ny] && grid[nx][ny] == '.') {
            visited[nx][ny] = 1;
            q.push({nx, ny});
        }
    }
}
```

---

# Final Practice Rule

For every problem, force yourself to type the data structure declaration manually.

Do not copy-paste these at first.

You are training your hands to remember:

```cpp
vector<vector<int>> grid(n, vector<int>(m, 0));
queue<pair<int, int>> q;
vector<vector<pair<int, int>>> adj(n + 1);
priority_queue<int, vector<int>, greater<int>> pq;
```

Once these stop feeling scary, the rest of C++ becomes much more manageable.
