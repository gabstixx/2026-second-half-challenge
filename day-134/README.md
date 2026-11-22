# Day 134/184 - November 11, 2026

## LeetCode 75: Combination Sum III

### Problem

Find all combinations of k numbers that add up to n, using only digits 1 to 9, each used at most once. No combination may repeat.

- Input: k = 3, n = 7 → Output: [[1,2,4]]
- Input: k = 3, n = 9 → Output: [[1,2,6],[1,3,5],[2,3,4]]

### Solution: Backtracking

```typescript
function combinationSum3(k: number, n: number): number[][] {
  const result: number[][] = [];
  const current: number[] = [];

  function backtrack(start: number, remaining: number): void {
    if (current.length === k) {
      if (remaining === 0) {
        result.push([...current]);
      }
      return;
    }

    for (let num = start; num <= 9; num++) {
      if (num > remaining) break;

      current.push(num);
      backtrack(num + 1, remaining - num);
      current.pop();
    }
  }

  backtrack(1, n);
  return result;
}
```

### What Each Part Does

- current: the combination being built, one shared array
- start: the smallest number allowed next
- remaining: how much is left to reach n
- Base case: when current has k numbers, save it only if remaining is 0, then return either way
- push, recurse, pop: choose a number, explore deeper, then undo the choice

### Trace: k = 3, n = 7

```
backtrack(1, 7), current = []
  push 1 → backtrack(2, 6)
    push 2 → backtrack(3, 4)
      push 3 → length 3, remaining 1 → discard
      pop 3
      push 4 → length 3, remaining 0 → save [1,2,4]
      pop 4
      5 > remaining 4 → break
    pop 2
    push 3 → backtrack(4, 3) → 4 > 3 → break
    ...

result = [[1,2,4]]
```

### Details

- Passing num + 1 means each digit is used at most once, and combinations only go in increasing order
- result.push([...current]) saves a copy. Pushing current itself would store a reference, and the later pop calls would empty it.
- if (num > remaining) break is an optimization. Numbers only get bigger, so once one is too large the rest are too.

### Compared to Letter Combinations

- Letter Combinations built strings, which are immutable, so there was nothing to undo
- Here one array is mutated, so pop is required after each recursive call
- That undo step is the "back" in backtracking

### Complexity

- Time: O(C(9, k) × k), at most C(9, k) combinations and copying each costs k
- Space: O(k) for the recursion depth and current, excluding the output

### Key Learning

Backtracking is choose, explore, undo.
When the state is mutable, the undo step must restore it exactly.

---

**Streak: 134/184** 🔥
