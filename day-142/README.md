# Day 142/184 - November 19, 2026

## LeetCode 75: House Robber

### Problem

nums[i] is the money in house i. You can't rob two adjacent houses. Find the maximum you can rob.

- Input: nums = [1,2,3,1]
- Output: 4 (rob houses 0 and 2: 1 + 3)
- Input: nums = [2,7,9,3,1]
- Output: 12 (rob houses 0, 2 and 4: 2 + 9 + 1)

### The Idea

dp[i] = the maximum money from the first i houses.

For the next house there are two choices:

```
skip it: dp[i-1]
rob it:  dp[i-2] + nums[i-1]   (the house right before it must be skipped)
```

```
dp[i] = max(dp[i-1], dp[i-2] + nums[i-1])
```

dp[0] = 0 (no houses) and dp[1] = nums[0].

### Version 1: DP Array

```typescript
function rob(nums: number[]): number {
  const n = nums.length;
  const dp: number[] = new Array(n + 1).fill(0);

  dp[0] = 0;
  dp[1] = nums[0];

  for (let i = 2; i <= n; i++) {
    dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
  }

  return dp[n];
}
```

- Time: O(n)
- Space: O(n)

### Version 2: Two Variables

```typescript
function rob(nums: number[]): number {
  let prev2 = 0;
  let prev1 = 0;

  for (const money of nums) {
    const current = Math.max(prev1, prev2 + money);
    prev2 = prev1;
    prev1 = current;
  }

  return prev1;
}
```

- Time: O(n)
- Space: O(1)

Each step only looks back two values, so the array isn't needed. prev2 is the best up to two houses back and prev1 is the best up to the previous house.

### Trace

```
nums = [2,7,9,3,1]

money = 2: current = max(0, 0+2)   = 2   → prev2 = 0,  prev1 = 2
money = 7: current = max(2, 0+7)   = 7   → prev2 = 2,  prev1 = 7
money = 9: current = max(7, 2+9)   = 11  → prev2 = 7,  prev1 = 11
money = 3: current = max(11, 7+3)  = 11  → prev2 = 11, prev1 = 11
money = 1: current = max(11, 11+1) = 12  → prev2 = 11, prev1 = 12

return 12
```

### Compared to the Last Two

- Tribonacci: adds the previous three values
- Min Cost Climbing Stairs: minimum of two options
- House Robber: maximum of two options (skip or rob)

Same loop shape each time. Only the choice inside it changes.

### Complexity

| Approach      | Time | Space |
| ------------- | ---- | ----- |
| DP array      | O(n) | O(n)  |
| Two variables | O(n) | O(1)  |

### Key Learning

Define what dp[i] means, then ask what the choices are at step i.
When a step only looks back a fixed number of values, replace the array with variables.

---

**Streak: 142/184** 🔥
