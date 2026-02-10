---
date: 2026-02-10
tags:
  - math
title: Integer Partition
description: Integer partition and solution
posted: https://blog.realbro.dev/posts/integer-partition
slug: integer-partition
---

### What is an integer partition?

I encountered an interesting problem called **integer partition** while reading _Math Girls 1_. It's about counting the ways to express a positive integer as a sum of positive integers, where order doesn't matter. The book introduces this using an analogy: how many ways can you pay N cents using coins of any denomination (1 cent, 2 cents, 3 cents, and so on)?

For example,

$$
\begin{align*}
P_1 = 1 &: \\
1 &= 1 \\
\\
P_2 = 2 & : \\
2 & = 2 \\
  & = 1 + 1 \\
\\
P_3 = 3 & : \\
3 & = 3 \\
  & = 2 + 1 \\
  & = 1 + 1 + 1 \\
\\
P_4 = 5 & : \\
4 & = 4 \\
  & = 3 + 1 \\
  & = 2 + 2 \\
  & = 2 + 1 + 1 \\
  & = 1 + 1 + 1 + 1 \\
\end{align*}
$$

### Solution

Although I tried to find a closed-form formula for $P_n$​, I failed. However, I discovered a recurrence relation that allows me to compute it using programming.

To derive this relation, let's use the coin payment analogy. We can divide the partitions of n into cases based on the largest number used. For instance, some partitions use nn n as their largest part, others use n-1 as their largest part, and so on down to partitions using only '1's.

To formalize this, I'll introduce a restricted partition $P_{n,k}$​, which counts the partitions of nn n using only integers less than or equal to k. Then the total number of partitions is:

$$
P_n = P_{n,n} \tag{1}
$$

The key insight is that $P_{n,k}$​ satisfies the following recurrence relation:

$$
P_{n,k} = P_{n-k, k} + P_{n, k-1} \quad with \space P_{0, 0} = 1,\,  P_{n, 0} = 0 \space for \space n>0 \tag{2}
$$

The logic is that partitions of n with maximum part k either use at least one k (in which case, removing one k gives a partition counted by $P_{n-k,k}$​) or don't use k at all (giving $P_{n,k-1}$).

### Actual Code

Based on recurrence relations (2), python code below can calculate $P_n$:

```python
"""
integer partition is a way of writing n as a sum of positive integers.
For example, 4 can be written as:
    4
    = 3 + 1
    = 2 + 2
    = 2 + 1 + 1
    = 1 + 1 + 1 + 1
where the partition number for 4 is 5.
"""


class Solution:
    """Solution for integer partition."""

    def __init__(self, _n):
        self._n = _n
        size = self._n + 1
        # cache is n+1 * n+1 matrix
        # where cache[i][j] means P(i, j), the partition number for i with max j.
        self._cache = [[None for _ in range(size)] for _ in range(size)]

    def get_partition(self):
        """get partition number for n."""
        return self._sub_partition(self._n, self._n)

    def _sub_partition(self, n, k):
        """P(n k), sub partition for n with max k."""

        # cache hit
        if self._cache[n][k] is not None:
            return self._cache[n][k]

        if k == 0:
            # P(n, 0) = 0 except P(0, 0) = 1
            self._cache[n][k] = 0 if n > 0 else 1
            return self._cache[n][k]

        if n < k:
            # n < k => P(n, k) = P(n, n)
            self._cache[n][k] = self._sub_partition(n, n)
        else:
            # n >= k => P(n, k) = P(n-k, k) + P(n, k-1)
            self._cache[n][k] = self._sub_partition(n - k, k) + self._sub_partition(
                n, k - 1
            )
        return self._cache[n][k]


def main():
    """main function."""
    N = 15
    for target in range(1, N + 1):
        print(f"partition number for {target} is {Solution(target).get_partition()}")


if __name__ == "__main__":
    main()

```
