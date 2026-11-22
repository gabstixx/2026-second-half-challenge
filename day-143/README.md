# Day 143/184 - November 20, 2026

## LeetCode 75: Domino and Tromino Tiling

### Problem

A 2 × n board can be tiled with two shapes, rotations allowed:

- a domino, a 2×1 rectangle
- a tromino, an L-shape covering 3 cells

Count the ways to cover the board completely, modulo 10⁹ + 7.

- Input: n = 3 → Output: 5
- Input: n = 4 → Output: 11

### Why It's Harder

The tromino leaves one cell sticking out, so the right edge of the tiling can be uneven.
Working out how those cases connect is the hard part.

### Where the Recurrence Comes From

Look at how the right end of a 2 × n board can be finished:

1. One vertical domino: what's left is a 2 × (n-1) board, so dp[n-1] ways
2. Two stacked horizontal dominoes: what's left is a 2 × (n-2) board, so dp[n-2] ways
3. Trominoes: an L-shape leaves a cell sticking out, which a chain of horizontal dominoes fills, ending in a second tromino. There are 2 mirror orientations and the chain can be any length, so this adds 2 × (dp[0] + dp[1] + ... + dp[n-3])

```
dp[n] = dp[n-1] + dp[n-2] + 2 × (dp[0] + ... + dp[n-3])
```

Write the same formula for dp[n-1] and subtract. Almost everything cancels:

```
dp[n] = 2 × dp[n-1] + dp[n-3]
```

Base cases:

- dp[0] = 1 (an empty board has one way)
- dp[1] = 1
- dp[2] = 2

### Solution

```typescript
function numTilings(n: number): number {
  const MOD = 1_000_000_007;

  if (n === 1) return 1;
  if (n === 2) return 2;

  const dp: number[] = new Array(n + 1).fill(0);
  dp[0] = 1;
  dp[1] = 1;
  dp[2] = 2;

  for (let i = 3; i <= n; i++) {
    dp[i] = (2 * dp[i - 1] + dp[i - 3]) % MOD;
  }

  return dp[n];
}
```

### Trace

```
dp[0] = 1, dp[1] = 1, dp[2] = 2

n = 3: 2 × 2 + 1  = 5
n = 4: 2 × 5 + 1  = 11
n = 5: 2 × 11 + 2 = 24
n = 6: 2 × 24 + 5 = 53
```

### Details

- % MOD is applied inside the loop. The numbers grow fast, so waiting until the end would overflow.
- n = 1 and n = 2 are handled before the loop, so the array always has room for dp[2].
- Each step only looks back three values, so the array can be replaced with three variables, as with Tribonacci.

### Complexity

- Time: O(n)
- Space: O(n), reducible to O(1)

### The Four 1D DP Problems

| Problem                   | Recurrence                           |
| ------------------------- | ------------------------------------ |
| N-th Tribonacci Number    | add the previous three values        |
| Min Cost Climbing Stairs  | minimum of two options               |
| House Robber              | maximum of two options (skip or rob) |
| Domino and Tromino Tiling | 2 × dp[n-1] + dp[n-3]                |

### Key Learning

Define what dp[i] means first. For harder problems, look at how the end of the structure can be finished and count each case.
When a formula has a long sum in it, writing it for n-1 and subtracting can collapse it into a short recurrence.

---

**Streak: 143/184** 🔥
