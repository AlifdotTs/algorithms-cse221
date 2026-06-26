# CSE221 Assignment 01 - Problems A to H: Explanation + C++ Solutions

Source: OCR cleaned from the uploaded Codeforces Gym PDF screenshot.

> Important for submission: the quiz blocks some keywords. The C++ codes below avoid the banned built-in arranging function and avoid the other forbidden API words inside the code blocks.

---

## A. Odd or Even?

### Problem idea
You are given `T` numbers. For each number, print whether it is odd or even.

### Key observation
A number is even when it is divisible by `2`.

So:

- `n % 2 == 0` means even
- otherwise odd

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;

    while (T--) {
        long long n;
        cin >> n;

        if (n % 2 == 0) {
            cout << n << " is an Even number.\n";
        } else {
            cout << n << " is an Odd number.\n";
        }
    }

    return 0;
}
```

### Complexity
`O(T)` time and `O(1)` extra memory.

---

## B. Calculator

### Problem idea
You are given `T` calculator expressions. Each line starts with the word `calculate`, followed by:

```txt
calculate a OP b
```

The operator can be:

```txt
+  -  *  /  %  &  |  ^  <<  >>
```

Division should produce floating-point output. Other operations are integer operations.

### Key observation
You do not need `eval`. Just read the operator as a string and use `if-else` conditions.

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;

    cout << fixed << setprecision(10);

    while (T--) {
        string word, op;
        long long a, b;

        cin >> word >> a >> op >> b;

        if (op == "+") {
            cout << a + b << '\n';
        } else if (op == "-") {
            cout << a - b << '\n';
        } else if (op == "*") {
            cout << a * b << '\n';
        } else if (op == "/") {
            cout << (long double)a / b << '\n';
        } else if (op == "%") {
            cout << a % b << '\n';
        } else if (op == "&") {
            cout << (a & b) << '\n';
        } else if (op == "|") {
            cout << (a | b) << '\n';
        } else if (op == "^") {
            cout << (a ^ b) << '\n';
        } else if (op == "<<") {
            cout << (a << b) << '\n';
        } else if (op == ">>") {
            cout << (a >> b) << '\n';
        }
    }

    return 0;
}
```

### Complexity
`O(T)` time and `O(1)` extra memory.

---

## C. Fast Sum

### Problem idea
For each test case, find:

```txt
1 + 2 + 3 + ... + N
```

The slow loop gives time limit issues for large `N`.

### Key observation
Use the formula:

```txt
sum = N * (N + 1) / 2
```

Use `long long`, because `N` can be large.

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;

    while (T--) {
        long long n;
        cin >> n;

        long long ans = n * (n + 1) / 2;
        cout << ans << '\n';
    }

    return 0;
}
```

### Complexity
`O(T)` time and `O(1)` extra memory.

---

## D. Is Sorted?

### Problem idea
For each array, check whether it is in non-decreasing order.

Non-decreasing means:

```txt
a[i] <= a[i + 1]
```

for every valid adjacent pair.

### Key observation
You only need to compare neighboring values. If any previous value is greater than the next value, answer is `NO`.

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;

    while (T--) {
        int n;
        cin >> n;

        vector<int> a(n);
        for (int i = 0; i < n; i++) {
            cin >> a[i];
        }

        bool good = true;

        for (int i = 0; i + 1 < n; i++) {
            if (a[i] > a[i + 1]) {
                good = false;
                break;
            }
        }

        if (good) cout << "YES\n";
        else cout << "NO\n";
    }

    return 0;
}
```

### Complexity
`O(N)` per test case and `O(N)` memory.

---

## E. Reverse Sorting

### Problem idea
You are given an array. In one operation, you can choose a subarray of length exactly `3` and reverse it.

Example operation on positions `i, i + 1, i + 2`:

```txt
[x, y, z] becomes [z, y, x]
```

So the middle element does not move. Only the first and third elements swap.

You need to decide whether the array can become non-decreasing, and if yes, print the operations.

### Key observation
Reversing a length-3 subarray swaps positions `i` and `i + 2`.

That means an element can only move by `2` positions at a time.

So:

- elements at odd positions can only move among odd positions
- elements at even positions can only move among even positions

Therefore, we can independently arrange the values at odd positions and even positions. After that, if the full array becomes non-decreasing, print `YES`; otherwise print `NO`.

### How to build operations
For positions with the same parity, adjacent movement in that parity-group means swapping actual positions `j - 2` and `j`.

That is exactly one valid length-3 reverse operation from:

```txt
j - 2 to j
```

Using this, we can use insertion-style adjacent swaps inside each parity group.

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;

    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    vector<pair<int, int>> moves;

    for (int p = 0; p < 2; p++) {
        for (int i = p + 2; i < n; i += 2) {
            int j = i;

            while (j - 2 >= p && a[j - 2] > a[j]) {
                int temp = a[j - 2];
                a[j - 2] = a[j];
                a[j] = temp;

                moves.push_back({j - 1, j + 1});
                j -= 2;
            }
        }
    }

    bool good = true;
    for (int i = 0; i + 1 < n; i++) {
        if (a[i] > a[i + 1]) {
            good = false;
            break;
        }
    }

    if (!good) {
        cout << "NO\n";
    } else {
        cout << "YES\n";
        cout << moves.size() << '\n';

        for (int i = 0; i < (int)moves.size(); i++) {
            cout << moves[i].first << " " << moves[i].second << '\n';
        }
    }

    return 0;
}
```

### Why this works
The operation can never move an odd-position element to an even position, or an even-position element to an odd position. So the best possible final form is made by arranging each parity-position group internally.

If that final merged array is not non-decreasing, then no other valid sequence can fix it.

### Complexity
`O(N^2)` time and `O(N^2)` operations in the worst case. `N <= 1000`, so this is fine.

---

## F. An Ancient Sorting Algorithm

### Problem idea
You are given an array. You may only swap adjacent elements if both have the same parity.

That means:

- even can swap with even
- odd can swap with odd
- even cannot swap with odd

After no more useful valid swaps are possible, print the final array.

### Key observation
Elements can only move inside continuous blocks of same parity.

Example:

```txt
4 2 4 | 7 1 | 6 | 1
```

The blocks are:

```txt
even even even | odd odd | even | odd
```

So each block can be arranged internally, but values cannot cross parity boundaries.

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;

    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    int start = 0;

    while (start < n) {
        int last = start;

        while (last + 1 < n && a[last] % 2 == a[last + 1] % 2) {
            last++;
        }

        for (int i = start + 1; i <= last; i++) {
            int key = a[i];
            int j = i - 1;

            while (j >= start && a[j] > key) {
                a[j + 1] = a[j];
                j--;
            }

            a[j + 1] = key;
        }

        start = last + 1;
    }

    for (int i = 0; i < n; i++) {
        cout << a[i] << " ";
    }

    return 0;
}
```

### Complexity
Worst case `O(N^2)` time and `O(1)` extra memory besides the array.

---

## G. Sorting Again??

### Problem idea
You have student IDs and marks. You need to rank students by:

1. Higher mark first
2. If marks are equal, lower ID first

You also need to print the minimum number of swaps needed to transform the original order into the ranked order.

### Key observation for minimum swaps
Once we know the final ranked order, this becomes a minimum swaps problem between two permutations.

For a cycle of size `k`, minimum swaps needed is:

```txt
k - 1
```

So total minimum swaps is the sum of `cycle_size - 1` over all cycles.

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Student {
    int id;
    int mark;
};

bool better(Student a, Student b) {
    if (a.mark != b.mark) return a.mark > b.mark;
    return a.id < b.id;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;

    while (T--) {
        int n;
        cin >> n;

        vector<Student> a(n), ranked(n);

        for (int i = 0; i < n; i++) {
            cin >> a[i].id;
        }

        for (int i = 0; i < n; i++) {
            cin >> a[i].mark;
        }

        ranked = a;

        for (int i = 1; i < n; i++) {
            Student key = ranked[i];
            int j = i - 1;

            while (j >= 0 && better(key, ranked[j])) {
                ranked[j + 1] = ranked[j];
                j--;
            }

            ranked[j + 1] = key;
        }

        int target[1005];
        for (int i = 0; i < n; i++) {
            target[ranked[i].id] = i;
        }

        vector<int> seen(n, 0);
        int swaps = 0;

        for (int i = 0; i < n; i++) {
            if (seen[i] || target[a[i].id] == i) {
                continue;
            }

            int count = 0;
            int j = i;

            while (!seen[j]) {
                seen[j] = 1;
                j = target[a[j].id];
                count++;
            }

            swaps += count - 1;
        }

        cout << "Minimum swaps: " << swaps << '\n';

        for (int i = 0; i < n; i++) {
            cout << "ID: " << ranked[i].id << " Mark: " << ranked[i].mark << '\n';
        }
    }

    return 0;
}
```

### Walkthrough of the sample
Original IDs:

```txt
7 4 9 3 2 5 1
```

Marks:

```txt
40 50 50 20 10 10 10
```

Final ranking:

```txt
4 9 7 3 1 2 5
```

The original-to-final position mapping forms cycles. The cycle method gives `4` swaps, which is the minimum.

### Complexity
`O(N^2)` per test case because of insertion-style arranging, plus `O(N)` for cycle counting.

---

## H. Trains?

### Problem idea
You are given train schedule lines like:

```txt
DhumketuExpress will departure for Chittagong at 02:30
```

You need to order them by:

1. Train name in lexicographical order
2. If train names are same, later departure time first
3. If both are same, keep the earlier input line first

### Key observation
Each line has a fixed structure. The train name is the first word. The time is the last word.

So we can parse:

- train name: first token
- time: last token, converted to minutes
- original full line: preserved for output
- input index: used to break complete ties

### C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Train {
    string name;
    string line;
    int minutes;
    int idx;
};

int getTimeValue(string s) {
    int h = 0;
    int m = 0;
    int i = 0;

    while (i < (int)s.size() && s[i] != ':') {
        h = h * 10 + (s[i] - '0');
        i++;
    }

    i++;

    while (i < (int)s.size()) {
        m = m * 10 + (s[i] - '0');
        i++;
    }

    return h * 60 + m;
}

bool beforeTrain(Train a, Train b) {
    if (a.name != b.name) return a.name < b.name;
    if (a.minutes != b.minutes) return a.minutes > b.minutes;
    return a.idx < b.idx;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    cin.ignore();

    vector<Train> a(n);

    for (int i = 0; i < n; i++) {
        getline(cin, a[i].line);
        a[i].idx = i;

        stringstream ss(a[i].line);
        ss >> a[i].name;

        string token;
        string last;

        while (ss >> token) {
            last = token;
        }

        a[i].minutes = getTimeValue(last);
    }

    for (int i = 1; i < n; i++) {
        Train key = a[i];
        int j = i - 1;

        while (j >= 0 && beforeTrain(key, a[j])) {
            a[j + 1] = a[j];
            j--;
        }

        a[j + 1] = key;
    }

    for (int i = 0; i < n; i++) {
        cout << a[i].line << '\n';
    }

    return 0;
}
```

### Complexity
`O(N^2)` time because of insertion-style arranging. Since `N <= 100`, this is more than enough.

---

# Quick Revision Summary

| Problem | Main idea |
|---|---|
| A | Check `n % 2` |
| B | Parse operator as string and handle cases manually |
| C | Use `N * (N + 1) / 2` |
| D | Compare adjacent elements |
| E | Length-3 reverse swaps same-index-parity positions only |
| F | Arrange each continuous same-parity block |
| G | Rank students, then count cycles for minimum swaps |
| H | Parse train name and time, then apply ranking rules |

