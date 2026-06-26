# C++ Syntax Deep Guide for Algorithm Labs

This is a **more descriptive version** of the C++ syntax drill guide.

The purpose is not to teach every corner of C++. The purpose is very specific:

> You should be able to write C++ for algorithm lab exams without freezing on syntax.

This guide focuses on:

- fast I/O
- data types
- loops
- `while (T--)`
- arrays and vectors
- higher-dimensional vectors
- strings
- pairs
- structs
- functions and methods
- passing by value/reference
- common data structures
- linked lists
- trees
- graphs
- recursion
- divide and conquer basics
- common syntax mistakes

You are coming from React / Next.js / Express, so your programming brain is not weak. Your brain is just used to a different style.

Framework-world thinking:

```text
components
objects
APIs
hooks
callbacks
async logic
database calls
```

Algorithm-world C++ thinking:

```text
variables
loops
arrays
vectors
structs
queues
stacks
graphs
trees
recursion
```

So your job is not to “become a C++ wizard.”  
Your job is to make algorithm C++ boring.

Boring code passes.

---

# 1. The Basic C++ Program

A normal lab-style C++ program looks like this:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    // your code here

    return 0;
}
```

Let’s break this down properly.

---

## 1.1 `#include <bits/stdc++.h>`

This line imports almost all standard C++ libraries commonly used in competitive programming and algorithm labs.

It includes things like:

```cpp
iostream
vector
string
queue
stack
map
set
algorithm
utility
cmath
climits
```

Because of it, you can use:

```cpp
cin
cout
vector
string
queue
stack
map
set
pair
priority_queue
```

without importing each header separately.

### Normal C++ style would be:

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <map>
#include <set>
using namespace std;
```

### Lab/contest style usually uses:

```cpp
#include <bits/stdc++.h>
using namespace std;
```

For your algorithm lab, use `bits/stdc++.h` unless the judge/compiler does not support it. Codeforces and most GNU C++ environments support it.

---

## 1.2 `using namespace std;`

C++ standard library items live inside the `std` namespace.

Without this:

```cpp
std::cout << "Hello";
std::vector<int> a;
std::string s;
```

With this:

```cpp
cout << "Hello";
vector<int> a;
string s;
```

For real production C++, people may avoid `using namespace std;`.

For lab exams, use it. It reduces syntax noise.

---

## 1.3 `int main()`

This is where your program starts.

```cpp
int main() {
    return 0;
}
```

`int` means the function returns an integer to the operating system.

`return 0;` usually means:

```text
program ended successfully
```

In online judges, forgetting `return 0;` usually still works in modern C++, but write it anyway because it is clean and familiar.

---

# 2. Fast I/O

Use this inside `main()`:

```cpp
ios::sync_with_stdio(false);
cin.tie(nullptr);
```

Full template:

```cpp
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    // code

    return 0;
}
```

---

## 2.1 Why do we need fast I/O?

C++ has two major input/output families:

1. C-style:
   ```cpp
   scanf
   printf
   ```

2. C++-style:
   ```cpp
   cin
   cout
   ```

By default, C++ keeps `cin/cout` synchronized with `scanf/printf`. That makes mixing them safer, but it slows `cin/cout`.

This line:

```cpp
ios::sync_with_stdio(false);
```

turns off that synchronization.

This line:

```cpp
cin.tie(nullptr);
```

unties `cin` from `cout`.

Normally, before reading input, `cout` may flush automatically. Untying avoids unnecessary flushing.

---

## 2.2 Important warning

If you use:

```cpp
ios::sync_with_stdio(false);
```

then do not mix:

```cpp
cin/cout
```

with:

```cpp
scanf/printf
```

In lab code, just use `cin` and `cout`.

---

## 2.3 `'\n'` vs `endl`

Use:

```cpp
cout << '\n';
```

Avoid:

```cpp
cout << endl;
```

Both print a newline, but `endl` also flushes output. Flushing repeatedly can slow down your program.

### Good:

```cpp
cout << answer << '\n';
```

### Avoid:

```cpp
cout << answer << endl;
```

---

# 3. Common Data Types

Data types tell C++ what kind of value a variable stores.

---

## 3.1 `int`

Stores normal integers.

```cpp
int x = 10;
```

Usually safe from around:

```text
-2,000,000,000 to +2,000,000,000
```

Use `int` for:

- array indexes
- small values
- node numbers
- counts
- loop variables

Example:

```cpp
int n;
cin >> n;

for (int i = 0; i < n; i++) {
    cout << i << '\n';
}
```

---

## 3.2 `long long`

Stores larger integers.

```cpp
long long x = 1000000000000LL;
```

Use `long long` for:

- large sums
- multiplication
- graph distances
- costs
- answers that can exceed `int`

Example:

```cpp
int n;
cin >> n;

vector<int> a(n);

for (int i = 0; i < n; i++) {
    cin >> a[i];
}

long long sum = 0;

for (int i = 0; i < n; i++) {
    sum += a[i];
}

cout << sum << '\n';
```

### Why not `int` for sum?

Suppose:

```text
n = 100000
a[i] = 100000
```

Sum is:

```text
100000 * 100000 = 10,000,000,000
```

That is bigger than `int`.

So use `long long`.

---

## 3.3 `double`

Stores decimal numbers.

```cpp
double price = 10.5;
```

Use for:

- averages
- geometry
- probability
- decimal calculations

Example:

```cpp
double average = (double)sum / n;
```

Why `(double)`?

If both `sum` and `n` are integers, C++ may do integer division.

```cpp
int a = 5;
int b = 2;

cout << a / b; // prints 2
```

But:

```cpp
cout << (double)a / b; // prints 2.5
```

---

## 3.4 `char`

Stores a single character.

```cpp
char c = 'A';
```

Use single quotes:

```cpp
'A'
'7'
'#'
'.'
```

Do not use double quotes for char.

Wrong:

```cpp
char c = "A";
```

Correct:

```cpp
char c = 'A';
```

---

## 3.5 `bool`

Stores true or false.

```cpp
bool found = false;
```

Example:

```cpp
bool found = false;

for (int i = 0; i < n; i++) {
    if (a[i] == target) {
        found = true;
        break;
    }
}

if (found) {
    cout << "YES\n";
}
else {
    cout << "NO\n";
}
```

---

## 3.6 `string`

Stores text.

```cpp
string name;
cin >> name;
```

This reads one word only.

Input:

```text
Alif Hassan
```

Code:

```cpp
string s;
cin >> s;
```

`s` becomes:

```text
Alif
```

To read a full line with spaces:

```cpp
string line;
getline(cin, line);
```

### Common `cin` + `getline` problem

If you do:

```cpp
int n;
cin >> n;

string line;
getline(cin, line);
```

`getline` may read the leftover newline after `n`.

Fix:

```cpp
cin.ignore();
getline(cin, line);
```

---

# 4. Variables and Scope

A variable exists inside the block `{ }` where it is declared.

Example:

```cpp
if (true) {
    int x = 10;
}

cout << x; // error
```

`x` only exists inside the `if` block.

---

## 4.1 Global variables

Variables declared outside all functions are global.

```cpp
int n;
vector<int> a;

int main() {
    cin >> n;
    a.resize(n);
}
```

Global variables are useful for recursion/DFS problems because helper functions can access them.

Example:

```cpp
vector<vector<int>> adj;
vector<int> visited;

void dfs(int u) {
    visited[u] = 1;

    for (int v : adj[u]) {
        if (!visited[v]) {
            dfs(v);
        }
    }
}
```

For lab exams, using globals for graph/DFS is okay.

---

# 5. Input and Output

---

## 5.1 Reading one value

```cpp
int n;
cin >> n;
```

---

## 5.2 Reading multiple values

```cpp
int a, b, c;
cin >> a >> b >> c;
```

Input:

```text
10 20 30
```

Now:

```text
a = 10
b = 20
c = 30
```

---

## 5.3 Reading an array

```cpp
int n;
cin >> n;

vector<int> a(n);

for (int i = 0; i < n; i++) {
    cin >> a[i];
}
```

---

## 5.4 Printing

```cpp
cout << "Hello\n";
```

Print variable:

```cpp
cout << n << '\n';
```

Print multiple values:

```cpp
cout << a << " " << b << '\n';
```

Formatted output style:

```cpp
cout << "ID: " << id << " Mark: " << mark << '\n';
```

---

# 6. Operators

---

## 6.1 Arithmetic operators

```cpp
+
-
*
/
%
```

Example:

```cpp
int a = 10;
int b = 3;

cout << a + b << '\n'; // 13
cout << a - b << '\n'; // 7
cout << a * b << '\n'; // 30
cout << a / b << '\n'; // 3
cout << a % b << '\n'; // 1
```

`%` gives the remainder.

Use `%` for parity:

```cpp
if (x % 2 == 0) {
    cout << "even\n";
}
else {
    cout << "odd\n";
}
```

---

## 6.2 Comparison operators

```cpp
==
!=
<
>
<=
>=
```

Example:

```cpp
if (a == b) {
    cout << "same\n";
}
```

Important: `==` checks equality.  
`=` assigns value.

Wrong:

```cpp
if (x = 5) {
}
```

Correct:

```cpp
if (x == 5) {
}
```

---

## 6.3 Logical operators

```cpp
&&  // and
||  // or
!   // not
```

Example:

```cpp
if (x >= 0 && x < n) {
    cout << "valid index\n";
}
```

---

# 7. Conditions

## 7.1 `if`

```cpp
if (x > 0) {
    cout << "positive\n";
}
```

## 7.2 `if-else`

```cpp
if (x % 2 == 0) {
    cout << "even\n";
}
else {
    cout << "odd\n";
}
```

## 7.3 `else if`

```cpp
if (mark >= 90) {
    cout << "A\n";
}
else if (mark >= 80) {
    cout << "B\n";
}
else {
    cout << "C or below\n";
}
```

---

# 8. Loops

Loops repeat code.

---

## 8.1 `for` loop

Use when you know how many times to run.

```cpp
for (int i = 0; i < n; i++) {
    cout << i << '\n';
}
```

Meaning:

```text
int i = 0       start
i < n           condition
i++             update after each loop
```

For `n = 5`, it prints:

```text
0
1
2
3
4
```

---

## 8.2 Reverse loop

```cpp
for (int i = n - 1; i >= 0; i--) {
    cout << a[i] << " ";
}
```

This goes from last index to first index.

If `n = 5`, indexes are:

```text
4 3 2 1 0
```

---

## 8.3 `while` loop

Use when you do not know exactly how many times it will run.

```cpp
while (x > 0) {
    cout << x << '\n';
    x--;
}
```

---

## 8.4 `while (T--)`

Very common in test case problems.

```cpp
int T;
cin >> T;

while (T--) {
    // solve one test case
}
```

### What does it mean?

`T--` is post-decrement.

It means:

```text
use current value of T
then decrease T by 1
```

Suppose:

```cpp
T = 3;
```

Then:

| Loop check | Current T used | Enter loop? | T after check |
|---|---:|---:|---:|
| 1 | 3 | yes | 2 |
| 2 | 2 | yes | 1 |
| 3 | 1 | yes | 0 |
| 4 | 0 | no | - |

So it runs exactly 3 times.

Equivalent:

```cpp
while (T > 0) {
    // solve
    T--;
}
```

Also equivalent:

```cpp
for (int tc = 0; tc < T; tc++) {
    // solve
}
```

### Which should you use?

If `while (T--)` confuses you under pressure, use:

```cpp
for (int tc = 1; tc <= T; tc++) {
    // solve
}
```

There is no prize for being fancy.

---

## 8.5 `break`

Stops the nearest loop.

```cpp
for (int i = 0; i < n; i++) {
    if (a[i] == target) {
        cout << "found\n";
        break;
    }
}
```

---

## 8.6 `continue`

Skips the rest of the current iteration.

```cpp
for (int i = 0; i < n; i++) {
    if (a[i] < 0) {
        continue;
    }

    cout << a[i] << " ";
}
```

This prints only non-negative values.

---

# 9. Arrays vs Vectors

---

## 9.1 C-style array

```cpp
int a[1000];
```

This creates a fixed-size array.

Problems:

- size must often be known
- less flexible
- harder to pass around safely

---

## 9.2 Vector

Prefer vector.

```cpp
vector<int> a(n);
```

A vector is a dynamic array.

It knows its own size and can grow.

---

## 9.3 Basic vector input

```cpp
int n;
cin >> n;

vector<int> a(n);

for (int i = 0; i < n; i++) {
    cin >> a[i];
}
```

---

## 9.4 Vector initialized with value

```cpp
vector<int> dist(n, -1);
```

This creates:

```text
n elements, all -1
```

Example:

```cpp
vector<int> visited(n + 1, 0);
```

Useful for graph problems.

---

## 9.5 `push_back`

If you do not know size in advance:

```cpp
vector<int> a;

a.push_back(10);
a.push_back(20);
a.push_back(30);
```

Now vector contains:

```text
10 20 30
```

---

## 9.6 `size`

```cpp
int len = a.size();
```

Better:

```cpp
int len = (int)a.size();
```

Because `a.size()` returns an unsigned type. Casting avoids annoying signed/unsigned warnings.

---

## 9.7 Common vector functions

```cpp
a.push_back(x);  // add x to end
a.pop_back();    // remove last element
a.back();        // last element
a.front();       // first element
a.size();        // number of elements
a.empty();       // true if size is 0
a.clear();       // remove all elements
```

---

## 9.8 Common vector mistake

Wrong:

```cpp
vector<int> a;
cin >> a[0]; // crash or undefined behavior
```

Why?

`a` has size 0. There is no `a[0]`.

Correct:

```cpp
vector<int> a(1);
cin >> a[0];
```

Or:

```cpp
vector<int> a;
int x;
cin >> x;
a.push_back(x);
```

---

# 10. Higher-Dimensional Vectors

This is one of your weak-point targets, so this section is intentionally detailed.

---

## 10.1 2D vector mental model

A 2D vector is like a table.

```text
row 0:  1  2  3
row 1:  4  5  6
row 2:  7  8  9
```

In C++:

```cpp
vector<vector<int>> grid(n, vector<int>(m, 0));
```

Meaning:

```text
make n rows
each row is a vector<int> of size m
each cell starts as 0
```

---

## 10.2 2D vector syntax breakdown

```cpp
vector<vector<int>> grid(n, vector<int>(m, 0));
```

Break it:

```cpp
vector<int>(m, 0)
```

means:

```text
one row with m integers, all 0
```

Then:

```cpp
vector<vector<int>>(n, vector<int>(m, 0))
```

means:

```text
n copies of that row
```

---

## 10.3 2D vector input

```cpp
int n, m;
cin >> n >> m;

vector<vector<int>> grid(n, vector<int>(m));

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        cin >> grid[i][j];
    }
}
```

Access:

```cpp
grid[i][j]
```

`i` is row.  
`j` is column.

---

## 10.4 2D visited grid

For grid BFS/DFS:

```cpp
vector<vector<int>> visited(n, vector<int>(m, 0));
```

Use:

```cpp
visited[x][y] = 1;
```

Check:

```cpp
if (!visited[x][y]) {
}
```

---

## 10.5 2D distance grid

For shortest path in grid:

```cpp
vector<vector<int>> dist(n, vector<int>(m, -1));
```

`-1` means unvisited/unreachable.

---

## 10.6 Vector of strings as grid

If input is:

```text
4 5
.....
.#...
..#..
.....
```

Use:

```cpp
int n, m;
cin >> n >> m;

vector<string> grid(n);

for (int i = 0; i < n; i++) {
    cin >> grid[i];
}
```

Access:

```cpp
grid[i][j]
```

This is usually easier for character grids.

Example:

```cpp
if (grid[i][j] == '#') {
    cout << "wall\n";
}
```

---

## 10.7 3D vector mental model

A 3D vector is useful when each cell has multiple states.

Example:

```text
grid position (x, y)
state 0 = have not used special move
state 1 = used special move
```

Then distance needs:

```cpp
dist[x][y][state]
```

Declaration:

```cpp
vector<vector<vector<int>>> dist(
    n,
    vector<vector<int>>(
        m,
        vector<int>(2, -1)
    )
);
```

---

## 10.8 3D vector syntax breakdown

Inside:

```cpp
vector<int>(2, -1)
```

Creates:

```text
[-1, -1]
```

Then:

```cpp
vector<vector<int>>(m, vector<int>(2, -1))
```

Creates:

```text
m columns, each column has 2 states
```

Then:

```cpp
vector<vector<vector<int>>>(n, ...)
```

Creates:

```text
n rows
```

So:

```cpp
dist[x][y][used]
```

means:

```text
row x
column y
state used
```

---

## 10.9 Common 3D vector usage

```cpp
int n, m;
cin >> n >> m;

vector<vector<vector<int>>> dist(n, vector<vector<int>>(m, vector<int>(2, -1)));
```

Set:

```cpp
dist[0][0][0] = 0;
```

Check:

```cpp
if (dist[nx][ny][used] == -1) {
    dist[nx][ny][used] = dist[x][y][used] + 1;
}
```

---

# 11. Strings

---

## 11.1 Reading string

```cpp
string s;
cin >> s;
```

Input:

```text
hello
```

---

## 11.2 String length

```cpp
int n = s.size();
```

or:

```cpp
int n = (int)s.length();
```

---

## 11.3 Access characters

```cpp
cout << s[0] << '\n';
```

Loop:

```cpp
for (int i = 0; i < (int)s.size(); i++) {
    cout << s[i] << '\n';
}
```

---

## 11.4 Modify character

```cpp
s[0] = 'A';
```

---

## 11.5 Add character/string

```cpp
s.push_back('x');
s += "abc";
```

---

# 12. Pair

A `pair` stores two values together.

```cpp
pair<int, int> p;
p.first = 10;
p.second = 20;
```

Or:

```cpp
pair<int, int> p = {10, 20};
```

---

## 12.1 Why use pair?

Use `pair` when two values naturally belong together:

- coordinate `(x, y)`
- edge `(neighbor, weight)`
- value and index
- distance and node

Example coordinate:

```cpp
pair<int, int> cell = {2, 3};

int x = cell.first;
int y = cell.second;
```

---

## 12.2 Vector of pairs

```cpp
vector<pair<int, int>> points;

points.push_back({1, 2});
points.push_back({3, 4});
```

Loop:

```cpp
for (pair<int, int> p : points) {
    cout << p.first << " " << p.second << '\n';
}
```

---

## 12.3 Queue of pairs

Useful for grid BFS:

```cpp
queue<pair<int, int>> q;

q.push({0, 0});

pair<int, int> cur = q.front();
q.pop();

int x = cur.first;
int y = cur.second;
```

---

# 13. Structs

A `struct` is a custom data type.

Think of it like a JavaScript object shape.

JavaScript:

```js
const student = {
  id: 4,
  mark: 50
};
```

C++:

```cpp
struct Student {
    int id;
    int mark;
};
```

Important:

```cpp
};
```

The semicolon after the closing brace is required.

---

## 13.1 Why use struct?

Use struct when a thing has multiple fields.

Examples:

Student:

```cpp
struct Student {
    int id;
    int mark;
};
```

Edge:

```cpp
struct Edge {
    int u;
    int v;
    int w;
};
```

Cell:

```cpp
struct Cell {
    int x;
    int y;
};
```

Tree node:

```cpp
struct Node {
    int value;
    Node* left;
    Node* right;
};
```

---

## 13.2 Creating a struct variable

```cpp
Student s;

s.id = 4;
s.mark = 50;
```

Access fields using dot:

```cpp
cout << s.id << " " << s.mark << '\n';
```

---

## 13.3 Vector of structs

```cpp
int n;
cin >> n;

vector<Student> students(n);
```

Input IDs:

```cpp
for (int i = 0; i < n; i++) {
    cin >> students[i].id;
}
```

Input marks:

```cpp
for (int i = 0; i < n; i++) {
    cin >> students[i].mark;
}
```

Print:

```cpp
for (int i = 0; i < n; i++) {
    cout << students[i].id << " " << students[i].mark << '\n';
}
```

---

## 13.4 Struct with constructor

You can create a constructor to initialize fields easily.

```cpp
struct Student {
    int id;
    int mark;

    Student(int _id, int _mark) {
        id = _id;
        mark = _mark;
    }
};
```

Use:

```cpp
Student s(4, 50);
```

For lab exams, constructors are optional. Simple assignment is often safer if you are rusty.

---

## 13.5 Struct with method

A method is a function inside a struct.

```cpp
struct Student {
    int id;
    int mark;

    void print() {
        cout << "ID: " << id << " Mark: " << mark << '\n';
    }
};
```

Use:

```cpp
Student s;
s.id = 4;
s.mark = 50;

s.print();
```

---

## 13.6 Struct comparison helper

For ranking:

```cpp
bool better(Student a, Student b) {
    if (a.mark != b.mark) {
        return a.mark > b.mark;
    }

    return a.id < b.id;
}
```

Meaning:

```text
a is better than b if:
1. a has higher mark
2. if marks equal, a has lower id
```

Use:

```cpp
if (better(students[j], students[best])) {
    best = j;
}
```

---

# 14. Functions

A function is reusable logic.

---

## 14.1 Function without return value

```cpp
void greet() {
    cout << "Hello\n";
}
```

Call:

```cpp
greet();
```

`void` means it returns nothing.

---

## 14.2 Function with return value

```cpp
int add(int a, int b) {
    return a + b;
}
```

Call:

```cpp
int result = add(5, 7);
cout << result << '\n';
```

---

## 14.3 Function parameters

```cpp
int square(int x) {
    return x * x;
}
```

Here, `x` is a parameter.

Call:

```cpp
cout << square(5) << '\n';
```

---

## 14.4 Passing vector by value

```cpp
void printVector(vector<int> a) {
    for (int x : a) {
        cout << x << " ";
    }
}
```

This copies the whole vector. For large vectors, this is slow.

---

## 14.5 Passing vector by reference

```cpp
void printVector(vector<int>& a) {
    for (int x : a) {
        cout << x << " ";
    }
}
```

`&` means the function receives the original vector, not a copy.

---

## 14.6 `const` reference

```cpp
void printVector(const vector<int>& a) {
    for (int x : a) {
        cout << x << " ";
    }
}
```

This means:

```text
do not copy the vector
also do not allow this function to modify it
```

Use `const vector<int>&` when the function only reads the vector.

---

# 15. Common Data Structures

---

## 15.1 Stack

Stack is last-in, first-out.

Real-life analogy:

```text
pile of plates
last plate placed is first plate removed
```

C++:

```cpp
stack<int> st;
```

Push:

```cpp
st.push(10);
st.push(20);
```

Top:

```cpp
cout << st.top() << '\n';
```

Pop:

```cpp
st.pop();
```

Check empty:

```cpp
if (st.empty()) {
    cout << "empty\n";
}
```

### Common stack use cases

- balanced parentheses
- undo behavior
- DFS
- next greater element

---

## 15.2 Queue

Queue is first-in, first-out.

Real-life analogy:

```text
line at a food counter
first person in line gets served first
```

C++:

```cpp
queue<int> q;
```

Push:

```cpp
q.push(10);
q.push(20);
```

Front:

```cpp
cout << q.front() << '\n';
```

Pop:

```cpp
q.pop();
```

Check empty:

```cpp
if (q.empty()) {
    cout << "empty\n";
}
```

### Common queue use cases

- BFS
- level order traversal
- simulation problems

---

## 15.3 Deque

Deque means double-ended queue.

You can add/remove from both ends.

```cpp
deque<int> dq;
```

Push back:

```cpp
dq.push_back(10);
```

Push front:

```cpp
dq.push_front(5);
```

Front/back:

```cpp
cout << dq.front() << " " << dq.back() << '\n';
```

Remove:

```cpp
dq.pop_front();
dq.pop_back();
```

Useful for:

- sliding window
- monotonic queue
- problems needing both ends

---

## 15.4 Priority Queue / Heap

A priority queue gives you the highest-priority item first.

By default, C++ priority queue is a max-heap.

```cpp
priority_queue<int> pq;
```

Example:

```cpp
pq.push(10);
pq.push(30);
pq.push(20);

cout << pq.top() << '\n'; // 30
```

Remove top:

```cpp
pq.pop();
```

---

## 15.5 Min-heap

For smallest value first:

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

This syntax means:

```text
priority_queue of int
stored internally using vector<int>
comparison is greater<int>, which makes it min-heap
```

---

## 15.6 Priority queue of pairs

Useful for Dijkstra:

```cpp
priority_queue<
    pair<long long, int>,
    vector<pair<long long, int>>,
    greater<pair<long long, int>>
> pq;
```

Push:

```cpp
pq.push({0, source});
```

Pop:

```cpp
pair<long long, int> cur = pq.top();
pq.pop();

long long distance = cur.first;
int node = cur.second;
```

Mental model:

```text
pair = {distance, node}
priority queue gives smallest distance first
```

---

## 15.7 Set

A set stores unique values in arranged order.

```cpp
set<int> s;

s.insert(5);
s.insert(2);
s.insert(5);
```

Set contains:

```text
2 5
```

No duplicates.

Loop:

```cpp
for (int x : s) {
    cout << x << " ";
}
```

Check existence:

```cpp
if (s.find(5) != s.end()) {
    cout << "found\n";
}
```

Remove:

```cpp
s.erase(5);
```

---

## 15.8 Map

A map stores key-value pairs.

Real-life analogy:

```text
student name -> mark
product id -> price
number -> frequency
```

C++:

```cpp
map<string, int> marks;
```

Use:

```cpp
marks["Alice"] = 90;
marks["Bob"] = 80;
```

Access:

```cpp
cout << marks["Alice"] << '\n';
```

Frequency count:

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

`p.first` is the key.  
`p.second` is the value.

---

# 16. Linked List Syntax

A linked list is a chain of nodes.

Each node stores:

```text
value
address of next node
```

C++ node:

```cpp
struct Node {
    int value;
    Node* next;
};
```

---

## 16.1 What is `Node*`?

`Node*` means pointer to a Node.

A pointer stores an address.

```cpp
Node* head;
```

This means:

```text
head can point to a Node
```

---

## 16.2 `nullptr`

If a pointer points to nothing:

```cpp
Node* head = nullptr;
```

`nullptr` means no node.

---

## 16.3 Create a node

```cpp
Node* newNode = new Node;

newNode->value = 10;
newNode->next = nullptr;
```

---

## 16.4 Why `->`?

If you have a normal object:

```cpp
Student s;
s.id = 5;
```

Use dot.

If you have a pointer:

```cpp
Node* node;
node->value = 10;
```

Use arrow.

This:

```cpp
node->value
```

is shorthand for:

```cpp
(*node).value
```

Meaning:

```text
go to the node this pointer points to, then access value
```

---

## 16.5 Insert at head

```cpp
Node* newNode = new Node;
newNode->value = x;
newNode->next = head;
head = newNode;
```

If list was:

```text
20 -> 30
```

and x is 10, after insert:

```text
10 -> 20 -> 30
```

---

## 16.6 Traverse linked list

```cpp
Node* current = head;

while (current != nullptr) {
    cout << current->value << " ";
    current = current->next;
}
```

Mental model:

```text
start at head
print current node
move to next node
stop when current is nullptr
```

---

# 17. Tree Syntax

A tree is made of nodes.

A binary tree node has:

```text
value
left child
right child
```

C++:

```cpp
struct Node {
    int value;
    Node* left;
    Node* right;
};
```

---

## 17.1 Create a node

```cpp
Node* createNode(int value) {
    Node* node = new Node;

    node->value = value;
    node->left = nullptr;
    node->right = nullptr;

    return node;
}
```

Use:

```cpp
Node* root = createNode(10);
root->left = createNode(5);
root->right = createNode(15);
```

This creates:

```text
      10
     /  \
    5    15
```

---

# 18. Graph Syntax

Graphs are nodes connected by edges.

---

## 18.1 Adjacency list

For `n` nodes:

```cpp
vector<vector<int>> adj(n + 1);
```

Why `n + 1`?

If nodes are numbered from `1` to `n`, you want indexes:

```text
1, 2, 3, ..., n
```

Index `0` is unused.

---

## 18.2 Undirected graph

If edge is between `u` and `v`, both can reach each other.

```cpp
adj[u].push_back(v);
adj[v].push_back(u);
```

Full input:

```cpp
int n, m;
cin >> n >> m;

vector<vector<int>> adj(n + 1);

for (int i = 0; i < m; i++) {
    int u, v;
    cin >> u >> v;

    adj[u].push_back(v);
    adj[v].push_back(u);
}
```

---

## 18.3 Directed graph

If edge goes from `u` to `v` only:

```cpp
adj[u].push_back(v);
```

Do not add reverse edge unless problem says undirected.

---

## 18.4 Weighted graph

If each edge has a weight/cost:

```cpp
vector<vector<pair<int, int>>> adj(n + 1);
```

Each pair is:

```text
{neighbor, weight}
```

Input:

```cpp
int u, v, w;
cin >> u >> v >> w;

adj[u].push_back({v, w});
adj[v].push_back({u, w});
```

Loop:

```cpp
for (pair<int, int> edge : adj[u]) {
    int v = edge.first;
    int w = edge.second;
}
```

---

# 19. Recursion Deep Explanation

This is one of your weak points, so this section is intentionally detailed.

Recursion means:

> A function solves a problem by calling itself on a smaller version of the same problem.

Every recursive function needs:

1. **Base case** — when to stop
2. **Recursive case** — how to reduce the problem

---

## 19.1 Simple recursion: countdown

```cpp
void countdown(int n) {
    if (n == 0) {
        return;
    }

    cout << n << '\n';
    countdown(n - 1);
}
```

Call:

```cpp
countdown(3);
```

Execution:

```text
countdown(3)
    print 3
    countdown(2)
        print 2
        countdown(1)
            print 1
            countdown(0)
                return
```

Output:

```text
3
2
1
```

---

## 19.2 What is a stack frame?

Every function call gets its own memory space called a stack frame.

When you call:

```cpp
countdown(3)
```

the call stack becomes:

```text
countdown(3)
```

Then it calls:

```cpp
countdown(2)
```

Stack:

```text
countdown(2)
countdown(3)
```

Then:

```text
countdown(1)
countdown(2)
countdown(3)
```

Then:

```text
countdown(0)
countdown(1)
countdown(2)
countdown(3)
```

When base case returns, calls finish one by one.

---

## 19.3 Recursion with return value

Example: factorial.

```cpp
int factorial(int n) {
    if (n == 0) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

Call:

```cpp
factorial(4)
```

Expands:

```text
4 * factorial(3)
4 * 3 * factorial(2)
4 * 3 * 2 * factorial(1)
4 * 3 * 2 * 1 * factorial(0)
4 * 3 * 2 * 1 * 1
24
```

---

## 19.4 Recursion with arrays

Sum array recursively:

```cpp
int sumArray(vector<int>& a, int index) {
    if (index == (int)a.size()) {
        return 0;
    }

    return a[index] + sumArray(a, index + 1);
}
```

For:

```text
a = [10, 20, 30]
```

Call:

```cpp
sumArray(a, 0)
```

Expands:

```text
a[0] + sumArray(a, 1)
10 + a[1] + sumArray(a, 2)
10 + 20 + a[2] + sumArray(a, 3)
10 + 20 + 30 + 0
60
```

Base case:

```cpp
if (index == a.size()) return 0;
```

Means:

```text
after the last index, no value left to add
```

---

## 19.5 Recursion with linked lists

Linked list:

```text
10 -> 20 -> 30 -> nullptr
```

Print normally:

```cpp
void printList(Node* head) {
    if (head == nullptr) {
        return;
    }

    cout << head->value << " ";
    printList(head->next);
}
```

Output:

```text
10 20 30
```

Print reverse:

```cpp
void printReverse(Node* head) {
    if (head == nullptr) {
        return;
    }

    printReverse(head->next);
    cout << head->value << " ";
}
```

Why reverse?

It first goes to the end:

```text
printReverse(10)
printReverse(20)
printReverse(30)
printReverse(nullptr)
```

Then prints while returning:

```text
30 20 10
```

---

## 19.6 Recursion with trees

Trees are naturally recursive.

A tree is:

```text
root
left subtree
right subtree
```

Each subtree is itself a tree.

Node:

```cpp
struct Node {
    int value;
    Node* left;
    Node* right;
};
```

---

## 19.7 Preorder traversal

Preorder means:

```text
current
left
right
```

Code:

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

For tree:

```text
      10
     /  \
    5    15
```

Output:

```text
10 5 15
```

---

## 19.8 Inorder traversal

Inorder means:

```text
left
current
right
```

Code:

```cpp
void inorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    inorder(root->left);
    cout << root->value << " ";
    inorder(root->right);
}
```

Output for tree:

```text
5 10 15
```

---

## 19.9 Postorder traversal

Postorder means:

```text
left
right
current
```

Code:

```cpp
void postorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    postorder(root->left);
    postorder(root->right);
    cout << root->value << " ";
}
```

Output:

```text
5 15 10
```

---

## 19.10 Count nodes in tree

```cpp
int countNodes(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    int leftCount = countNodes(root->left);
    int rightCount = countNodes(root->right);

    return 1 + leftCount + rightCount;
}
```

Meaning:

```text
number of nodes =
1 current node
+ nodes in left subtree
+ nodes in right subtree
```

This is the most important recursion pattern for trees.

---

## 19.11 Sum of tree

```cpp
int sumTree(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    int leftSum = sumTree(root->left);
    int rightSum = sumTree(root->right);

    return root->value + leftSum + rightSum;
}
```

Meaning:

```text
sum of tree =
current value
+ sum of left subtree
+ sum of right subtree
```

---

## 19.12 Height of tree

Height is longest path from root to leaf.

```cpp
int height(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    int leftHeight = height(root->left);
    int rightHeight = height(root->right);

    return 1 + max(leftHeight, rightHeight);
}
```

Meaning:

```text
height =
1 current level
+ larger height between left and right
```

---

## 19.13 Search in tree

```cpp
bool searchTree(Node* root, int target) {
    if (root == nullptr) {
        return false;
    }

    if (root->value == target) {
        return true;
    }

    bool foundLeft = searchTree(root->left, target);
    bool foundRight = searchTree(root->right, target);

    return foundLeft || foundRight;
}
```

Meaning:

```text
found if:
current node is target
or target exists in left subtree
or target exists in right subtree
```

---

## 19.14 Count leaf nodes

Leaf means node with no children.

```cpp
int countLeaves(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    if (root->left == nullptr && root->right == nullptr) {
        return 1;
    }

    return countLeaves(root->left) + countLeaves(root->right);
}
```

---

## 19.15 Real-life recursion examples

### Folder system

A folder contains files and folders.

```text
countFiles(folder):
    total = number of direct files
    for each subfolder:
        total += countFiles(subfolder)
```

### Company hierarchy

A manager has employees. Some employees manage others.

```text
countPeople(person):
    total = 1
    for each subordinate:
        total += countPeople(subordinate)
```

### Comment thread

A comment can have replies.

```text
printComment(comment):
    print current comment
    for each reply:
        printComment(reply)
```

### Website DOM

HTML elements contain other elements.

```text
search(element):
    if current element is target:
        return true
    search all children
```

Trees, folder systems, comment threads, and org charts all have the same recursive shape.

---

# 20. Divide and Conquer

Divide and conquer means:

```text
divide problem into smaller parts
solve each smaller part
combine answers
```

Most divide-and-conquer functions look like this:

```cpp
int solve(vector<int>& a, int left, int right) {
    if (left == right) {
        return a[left];
    }

    int mid = left + (right - left) / 2;

    int leftAns = solve(a, left, mid);
    int rightAns = solve(a, mid + 1, right);

    return max(leftAns, rightAns);
}
```

This finds max in a range.

---

## 20.1 Why `mid = left + (right - left) / 2`?

You may see:

```cpp
int mid = (left + right) / 2;
```

This usually works for small numbers.

But safer:

```cpp
int mid = left + (right - left) / 2;
```

Because `left + right` can overflow if both are huge.

---

## 20.2 Divide and conquer mental model

Example: find max in array.

```text
max of whole array =
max(
    max of left half,
    max of right half
)
```

That gives:

```cpp
int leftMax = solve(a, left, mid);
int rightMax = solve(a, mid + 1, right);

return max(leftMax, rightMax);
```

---

# 21. Manual Basic Arrangement

If built-in sorting is banned, use manual logic.

---

## 21.1 Selection-style arrangement

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

Meaning:

```text
for every position i:
    find smallest value from i to end
    place it at i
```

This is simple and easy to write.

---

## 21.2 Insertion-style arrangement

```cpp
for (int i = 1; i < n; i++) {
    int key = a[i];
    int j = i - 1;

    while (j >= 0 && a[j] > key) {
        a[j + 1] = a[j];
        j--;
    }

    a[j + 1] = key;
}
```

Meaning:

```text
take current value
shift bigger values right
insert current value in correct place
```

---

# 22. BFS Skeleton

BFS uses a queue.

For graph:

```cpp
vector<int> visited(n + 1, 0);
queue<int> q;

visited[source] = 1;
q.push(source);

while (!q.empty()) {
    int u = q.front();
    q.pop();

    for (int v : adj[u]) {
        if (!visited[v]) {
            visited[v] = 1;
            q.push(v);
        }
    }
}
```

Mental model:

```text
visit source
push source into queue
while queue has nodes:
    take front node
    push all unvisited neighbors
```

---

# 23. Grid BFS Skeleton

Direction arrays:

```cpp
int dx[] = {-1, 1, 0, 0};
int dy[] = {0, 0, -1, 1};
```

Validity function:

```cpp
bool valid(int x, int y, int n, int m) {
    return x >= 0 && x < n && y >= 0 && y < m;
}
```

BFS:

```cpp
queue<pair<int, int>> q;
vector<vector<int>> visited(n, vector<int>(m, 0));

visited[sx][sy] = 1;
q.push({sx, sy});

while (!q.empty()) {
    pair<int, int> cur = q.front();
    q.pop();

    int x = cur.first;
    int y = cur.second;

    for (int k = 0; k < 4; k++) {
        int nx = x + dx[k];
        int ny = y + dy[k];

        if (valid(nx, ny, n, m) && !visited[nx][ny] && grid[nx][ny] != '#') {
            visited[nx][ny] = 1;
            q.push({nx, ny});
        }
    }
}
```

---

# 24. Syntax Checklist Before Submit

Check:

- Did every statement end with `;`?
- Did every struct end with `};`?
- Did I include `#include <bits/stdc++.h>`?
- Did I write `using namespace std;`?
- Did I write fast I/O inside `main()`?
- Did I use `long long` for large sum/cost/distance?
- Did I initialize vector size before using indexes?
- Did I use `n + 1` for 1-based graph nodes?
- Did I reset variables inside each test case?
- Did I print newline after each test case?
- Did I confuse `=` and `==`?
- Did I confuse `.` and `->`?
- Did I check `nullptr` before accessing pointer fields?
- For recursion: do I have a base case?
- For grid: did I check boundaries?
- For graph: did I add both directions only if undirected?

---

# 25. Things To Memorize Cold

These should become muscle memory.

```cpp
vector<int> a(n);
vector<long long> dist(n + 1, INF);
vector<vector<int>> grid(n, vector<int>(m, 0));
vector<vector<int>> visited(n, vector<int>(m, 0));
vector<string> grid(n);
vector<pair<int, int>> points;
vector<vector<pair<int, int>>> adj(n + 1);

queue<int> q;
queue<pair<int, int>> q;
stack<int> st;

priority_queue<int> pq;
priority_queue<int, vector<int>, greater<int>> pq;

map<int, int> freq;
set<int> seen;
```

Structs:

```cpp
struct Student {
    int id;
    int mark;
};
```

```cpp
struct Edge {
    int u;
    int v;
    int w;
};
```

Linked list:

```cpp
struct Node {
    int value;
    Node* next;
};
```

Tree:

```cpp
struct Node {
    int value;
    Node* left;
    Node* right;
};
```

---

# Final Reminder

You are not trying to learn every algorithm immediately.

Your current mission is:

> I can write C++ syntax for algorithm data structures without choking.

Once syntax becomes boring, algorithm practice becomes much less painful.
