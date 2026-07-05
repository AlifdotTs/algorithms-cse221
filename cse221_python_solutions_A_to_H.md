# CSE221 Python Solutions: A to H

These are standalone Python solutions. For each problem, copy only the code block for that specific problem.

---

## A. Two Sum Trouble

```python
import sys

def main():
    input = sys.stdin.readline

    n, s = map(int, input().split())
    a = list(map(int, input().split()))

    l = 0
    r = n - 1

    while l < r:
        total = a[l] + a[r]

        if total == s:
            print(l + 1, r + 1)
            return
        elif total < s:
            l += 1
        else:
            r -= 1

    print(-1)

main()
```

---

## B. Two Sum Revisited

```python
import sys

def main():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    i = 0
    j = m - 1

    best_diff = float("inf")
    ans_i = 0
    ans_j = 0

    while i < n and j >= 0:
        total = a[i] + b[j]
        diff = abs(total - k)

        if diff < best_diff:
            best_diff = diff
            ans_i = i
            ans_j = j

        if total < k:
            i += 1
        elif total > k:
            j -= 1
        else:
            break

    print(ans_i + 1, ans_j + 1)

main()
```

---

## C. A Beautiful Sorted List

No `sort()` is used.

```python
import sys

def main():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    m = int(input())
    b = list(map(int, input().split()))

    i = 0
    j = 0
    merged = []

    while i < n and j < m:
        if a[i] <= b[j]:
            merged.append(a[i])
            i += 1
        else:
            merged.append(b[j])
            j += 1

    while i < n:
        merged.append(a[i])
        i += 1

    while j < m:
        merged.append(b[j])
        j += 1

    print(*merged)

main()
```

---

## D. Longest Subarray Sum

Since all numbers are positive, sliding window works.

```python
import sys

def main():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    answer = 0

    for right in range(n):
        current_sum += a[right]

        while current_sum > k:
            current_sum -= a[left]
            left += 1

        answer = max(answer, right - left + 1)

    print(answer)

main()
```

---

## E. Longest K-Distinct Subarray

```python
import sys

def main():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    left = 0
    answer = 0

    for right in range(n):
        x = a[right]
        freq[x] = freq.get(x, 0) + 1

        while len(freq) > k:
            y = a[left]
            freq[y] -= 1

            if freq[y] == 0:
                del freq[y]

            left += 1

        answer = max(answer, right - left + 1)

    print(answer)

main()
```

---

## F. Count the Numbers

Manual binary search is used.

```python
import sys

def lower_bound(a, target):
    left = 0
    right = len(a)

    while left < right:
        mid = (left + right) // 2

        if a[mid] < target:
            left = mid + 1
        else:
            right = mid

    return left


def upper_bound(a, target):
    left = 0
    right = len(a)

    while left < right:
        mid = (left + right) // 2

        if a[mid] <= target:
            left = mid + 1
        else:
            right = mid

    return left


def main():
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    output = []

    for _ in range(q):
        x, y = map(int, input().split())

        left_index = lower_bound(a, x)
        right_index = upper_bound(a, y)

        count = right_index - left_index
        output.append(str(count))

    print("\n".join(output))

main()
```

---

## G. Can You Split the Array?

Binary search on the answer.

```python
import sys

def can_split(a, k, max_allowed_sum):
    subarray_count = 1
    current_sum = 0

    for x in a:
        if current_sum + x <= max_allowed_sum:
            current_sum += x
        else:
            subarray_count += 1
            current_sum = x

    return subarray_count <= k


def main():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    left = max(a)
    right = sum(a)
    answer = right

    while left <= right:
        mid = (left + right) // 2

        if can_split(a, k, mid):
            answer = mid
            right = mid - 1
        else:
            left = mid + 1

    print(answer)

main()
```

### Note on Negative Numbers

If negative numbers are allowed, this exact greedy check no longer works reliably because adding more elements can decrease the subarray sum. The binary search plus greedy solution depends on all array elements being positive.

---

## H. Can You Cut the Ropes?

Binary search on rope length.

```python
import sys

def can_cut(ropes, k, length):
    pieces = 0

    for rope in ropes:
        pieces += rope // length

        if pieces >= k:
            return True

    return False


def main():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    ropes = list(map(int, input().split()))

    left = 1
    right = max(ropes)
    answer = -1

    while left <= right:
        mid = (left + right) // 2

        if can_cut(ropes, k, mid):
            answer = mid
            left = mid + 1
        else:
            right = mid - 1

    print(answer)

main()
```

---

## Pattern Summary

| Problem | Pattern |
|---|---|
| A | Two pointers |
| B | Two pointers |
| C | Merge two sorted arrays |
| D | Sliding window |
| E | Sliding window with frequency dictionary |
| F | Binary search bounds |
| G | Binary search on answer |
| H | Binary search on answer |
