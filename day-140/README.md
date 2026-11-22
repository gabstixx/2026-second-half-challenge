# Day 140/184 - November 17, 2026

## LeetCode 75: Min Cost Climbing Stairs

### Problem

cost[i] is the price of stair i. After paying, you can climb one or two stairs. You can start on stair 0 or 1. Find the minimum cost to reach the top, which is one position past the last stair.

- Input: cost = [10,15,20]
- Output: 15
- Input: cost = [1,100,1,1,1,100,1,1,100,1]
- Output: 6

### The Idea

dp[i] = the minimum cost to arrive at position i.

You reach i from i-1 or from i-2, paying the cost of the stair you leave:

```
dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
```

dp[0] = 0 and dp[1] = 0, since starting on either stair is free.

Two details:

- The top is at index n and is not a stair, so it has no cost
- You pay when you leave a stair, not when you land on it

### Version 1: DP Array

```typescript
function minCostClimbingStairs(cost: number[]): number {
  const n = cost.length;
  const dp: number[] = new Array(n + 1).fill(0);

  dp[0] = 0;
  dp[1] = 0;

  for (let i = 2; i <= n; i++) {
    dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
  }

  return dp[n];
}
```

- Time: O(n)
- Space: O(n)

### Version 2: Two Variables (Final)

```typescript
function minCostClimbingStairs(cost: number[]): number {
  let prev2 = 0;
  let prev1 = 0;

  for (let i = 2; i <= cost.length; i++) {
    const current = Math.min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
    prev2 = prev1;
    prev1 = current;
  }

  return prev1;
}
```

- Time: O(n)
- Space: O(1)

Each step only looks back two values, so the array isn't needed. prev2 is dp[i-2] and prev1 is dp[i-1].

### Trace

```
cost = [10, 15, 20]

prev2 = 0, prev1 = 0

i = 2: current = min(0 + cost[1], 0 + cost[0]) = min(15, 10) = 10
       prev2 = 0, prev1 = 10

i = 3: current = min(10 + cost[2], 0 + cost[1]) = min(30, 15) = 15
       prev2 = 10, prev1 = 15

return prev1 = 15
```

### Compared to Tribonacci

- Tribonacci adds the previous three values
- This takes the minimum of two options
- Same shape, a loop where each value is built from earlier ones

### Complexity

| Approach      | Time | Space |
| ------------- | ---- | ----- |
| DP array      | O(n) | O(n)  |
| Two variables | O(n) | O(1)  |

### Key Learning

Define what dp[i] means first, then the recurrence follows.
When a step only looks back a fixed number of values, the array can be replaced with variables.

---

**Streak: 140/184** 🔥
