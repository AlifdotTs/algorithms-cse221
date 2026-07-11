# CSE221 Assignment 03 — Python Solutions

All solutions use standard input/output and avoid the quiz's forbidden functions and modules.

---

## A. Count the Inversion

```python
import sys


def merge_sort(arr):
    n = len(arr)

    if n <= 1:
        return arr, 0

    mid = n // 2

    left, left_count = merge_sort(arr[:mid])
    right, right_count = merge_sort(arr[mid:])

    merged = []
    i = 0
    j = 0
    cross_count = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            cross_count += len(left) - i
            j += 1

    merged.extend(left[i:])
    merged.extend(right[j:])

    total_count = left_count + right_count + cross_count
    return merged, total_count


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    n = data[0]
    arr = data[1:1 + n]

    sorted_arr, inversion_count = merge_sort(arr)

    print(inversion_count)
    print(*sorted_arr)


if __name__ == "__main__":
    main()
```

**Complexity:** `O(N log N)` time and `O(N)` extra memory.

---

## B. Count the Inversion Revisited

Counts pairs `(i, j)` where `i < j` and:

```text
A[i] > A[j]²
```

```python
import sys
from bisect import bisect_left


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    n = data[0]
    arr = data[1:1 + n]

    squared_values = sorted({value * value for value in arr})
    size = len(squared_values)

    bit = [0] * (size + 1)

    def update(index):
        index += 1

        while index <= size:
            bit[index] += 1
            index += index & -index

    def query(end):
        total = 0

        while end > 0:
            total += bit[end]
            end -= end & -end

        return total

    answer = 0

    for i in range(n - 1, -1, -1):
        # Number of inserted squares strictly smaller than arr[i].
        position = bisect_left(squared_values, arr[i])
        answer += query(position)

        square = arr[i] * arr[i]
        square_position = bisect_left(squared_values, square)
        update(square_position)

    print(answer)


if __name__ == "__main__":
    main()
```

**Complexity:** `O(N log N)` time and `O(N)` extra memory.

---

## C. Fast Power Drift

Calculates:

```text
aᵇ mod 10⁷
```

```python
import sys


MOD = 10_000_000


def fast_exponent(base, exponent):
    result = 1
    base %= MOD

    while exponent > 0:
        if exponent & 1:
            result = (result * base) % MOD

        base = (base * base) % MOD
        exponent >>= 1

    return result


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    a = data[0]
    b = data[1]

    print(fast_exponent(a, b))


if __name__ == "__main__":
    main()
```

**Complexity:** `O(log b)` time and `O(1)` memory.

---

## D. Fast Matrix Drift

```python
import sys


MOD = 1_000_000_007


def matrix_exponent(a, b, c, d, exponent):
    r00, r01 = 1, 0
    r10, r11 = 0, 1

    while exponent > 0:
        if exponent & 1:
            n00 = (r00 * a + r01 * c) % MOD
            n01 = (r00 * b + r01 * d) % MOD
            n10 = (r10 * a + r11 * c) % MOD
            n11 = (r10 * b + r11 * d) % MOD

            r00, r01 = n00, n01
            r10, r11 = n10, n11

        n00 = (a * a + b * c) % MOD
        n01 = (a * b + b * d) % MOD
        n10 = (c * a + d * c) % MOD
        n11 = (c * b + d * d) % MOD

        a, b = n00, n01
        c, d = n10, n11

        exponent >>= 1

    return r00, r01, r10, r11


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    test_cases = data[0]
    index = 1
    output = []

    for _ in range(test_cases):
        a = data[index] % MOD
        b = data[index + 1] % MOD
        c = data[index + 2] % MOD
        d = data[index + 3] % MOD
        exponent = data[index + 4]
        index += 5

        r00, r01, r10, r11 = matrix_exponent(
            a, b, c, d, exponent
        )

        output.append(f"{r00} {r01}")
        output.append(f"{r10} {r11}")

    sys.stdout.write("\n".join(output))


if __name__ == "__main__":
    main()
```

**Complexity:** `O(T log X)` time and `O(1)` extra memory per test case.

---

## E. Fast Series Drift

Calculates:

```text
(a¹ + a² + ... + aⁿ) mod m
```

```python
import sys


def series_sum(a, n, mod):
    result_term = 1 % mod
    result_sum = 0

    block_term = a % mod
    block_sum = a % mod

    while n > 0:
        if n & 1:
            result_sum = (
                result_sum + result_term * block_sum
            ) % mod

            result_term = (
                result_term * block_term
            ) % mod

        block_sum = (
            block_sum * (1 + block_term)
        ) % mod

        block_term = (
            block_term * block_term
        ) % mod

        n >>= 1

    return result_sum


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    test_cases = data[0]
    index = 1
    output = []

    for _ in range(test_cases):
        a = data[index]
        n = data[index + 1]
        mod = data[index + 2]
        index += 3

        output.append(str(series_sum(a, n, mod)))

    sys.stdout.write("\n".join(output))


if __name__ == "__main__":
    main()
```

**Complexity:** `O(T log N)` time and `O(1)` extra memory per test case.

---

## F. Ordering Binary Tree

```python
import sys
from collections import deque


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    n = data[0]
    arr = data[1:1 + n]

    answer = []
    ranges = deque([(0, n - 1)])

    while ranges:
        left, right = ranges.popleft()

        if left > right:
            continue

        mid = (left + right) // 2
        answer.append(arr[mid])

        ranges.append((left, mid - 1))
        ranges.append((mid + 1, right))

    print(*answer)


if __name__ == "__main__":
    main()
```

**Complexity:** `O(N)` time and `O(N)` extra memory.

---

## G. 220 Trees

Given in-order and pre-order traversals, prints post-order.

```python
import sys


sys.setrecursionlimit(5000)


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    n = data[0]
    inorder = data[1:1 + n]
    preorder = data[1 + n:1 + 2 * n]

    position = {}

    for i in range(n):
        position[inorder[i]] = i

    answer = []
    preorder_index = 0

    def build(left, right):
        nonlocal preorder_index

        if left > right:
            return

        root = preorder[preorder_index]
        preorder_index += 1

        root_index = position[root]

        build(left, root_index - 1)
        build(root_index + 1, right)

        answer.append(root)

    build(0, n - 1)

    print(*answer)


if __name__ == "__main__":
    main()
```

**Complexity:** `O(N)` time and `O(N)` extra memory.

---

## H. 220 Trees Reassessed

Given in-order and post-order traversals, prints pre-order.

```python
import sys


sys.setrecursionlimit(5000)


def main():
    data = list(map(int, sys.stdin.buffer.read().split()))

    n = data[0]
    inorder = data[1:1 + n]
    postorder = data[1 + n:1 + 2 * n]

    position = {}

    for i in range(n):
        position[inorder[i]] = i

    answer = []

    def build(in_left, in_right, post_left, post_right):
        if in_left > in_right:
            return

        root = postorder[post_right]
        answer.append(root)

        root_index = position[root]
        left_size = root_index - in_left

        build(
            in_left,
            root_index - 1,
            post_left,
            post_left + left_size - 1
        )

        build(
            root_index + 1,
            in_right,
            post_left + left_size,
            post_right - 1
        )

    build(0, n - 1, 0, n - 1)

    print(*answer)


if __name__ == "__main__":
    main()
```

**Complexity:** `O(N)` time and `O(N)` extra memory.
