# Day 139/184 - November 16, 2026

## LeetCode 75: N-th Tribonacci Number

### Problem

T0 = 0, T1 = 1, T2 = 1, and Tn = Tn-1 + Tn-2 + Tn-3. Return the n-th Tribonacci number.

- Input: n = 4
- Output: 4
- Input: n = 25
- Output: 1389537

### First Attempt: Fibonacci by Mistake (Wrong)

```typescript
function tribonacci(n: number): number {
  if (n <= 1) {
    return n;
  }

  let dp = [];
  while (dp.length <= n + 1) {
    dp.push(0);
  }
  dp[1] = 1;

  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }

  return dp[n];
}
```

I read "Tribonacci" as "Fibonacci", so this adds only the previous two numbers.
For n = 4 it returns 3 instead of 4, so it failed on the first example.

### Solution: Bottom-Up DP

```typescript
function tribonacci(n: number): number {
  if (n === 0) return 0;
  if (n <= 2) return 1;

  const dp: number[] = new Array(n + 1).fill(0);
  dp[1] = 1;
  dp[2] = 1;

  for (let i = 3; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3];
  }

  return dp[n];
}
```

### Trace

```
n = 4

dp = [0, 1, 1, 0, 0]

i = 3: dp[3] = dp[2] + dp[1] + dp[0] = 1 + 1 + 0 = 2
i = 4: dp[4] = dp[3] + dp[2] + dp[1] = 2 + 1 + 1 = 4

return dp[4] = 4
```

### Why DP

Plain recursion recomputes the same values many times, which is exponential.
Storing each answer once and building up from the base cases makes it one loop.

### Details

- Base cases: n = 0 returns 0, and n of 1 or 2 returns 1
- The loop starts at 3, since dp[0], dp[1] and dp[2] are already known
- The recurrence adds three previous values, not two

### Complexity

- Time: O(n)
- Space: O(n)

### Space Optimization

Only the last three values are ever used, so the array can be replaced with three variables:

```typescript
function tribonacci(n: number): number {
  if (n === 0) return 0;
  if (n <= 2) return 1;

  let a = 0,
    b = 1,
    c = 1;

  for (let i = 3; i <= n; i++) {
    const next = a + b + c;
    a = b;
    b = c;
    c = next;
  }

  return c;
}
```

- Time: O(n)
- Space: O(1)

### Key Learning

Read the problem name and the recurrence twice before coding.
A clean solution can still answer the wrong question.
DP means building each answer from the smaller ones before it.

---

**Streak: 139/184** 🔥
