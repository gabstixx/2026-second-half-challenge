# Day 126/184 - November 3, 2026

## LeetCode 75: Successful Pairs of Spells and Potions

### Problem

A spell and potion form a successful pair if spell × potion >= success.
For each spell, count the potions that make a successful pair.

- Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7
- Output: [4,0,3]

### First Approach: Brute Force

```typescript
function successfulPairs(
  spells: number[],
  potions: number[],
  success: number,
): number[] {
  const len = spells.length;
  const res = spells.map((a) => potions.map((b) => a * b));
  let pairs: number[] = [];

  for (let i = 0; i < len; i++) {
    let result = res[i].filter((num) => num >= success);
    pairs.push(result.length);
  }

  return pairs;
}
```

Correct logic, but it builds the full n × m matrix of products.
On much larger and longer arrays it filled up memory.

- Time: O(n × m)
- Space: O(n × m)

### Second Approach: Sort + Binary Search

```typescript
function successfulPairs(
  spells: number[],
  potions: number[],
  success: number,
): number[] {
  const m = potions.length;
  potions.sort((a, b) => a - b);
  const pairs: number[] = [];

  for (const spell of spells) {
    const limit = Math.ceil(success / spell);
    let left = 0;
    let right = m;

    while (left < right) {
      const mid = Math.floor((left + right) / 2);

      if (potions[mid] >= limit) {
        right = mid;
      } else {
        left = mid + 1;
      }
    }

    pairs.push(m - left);
  }

  return pairs;
}
```

### Why It Works

spell × potion >= success is the same as potion >= success / spell.
With potions sorted, every potion from some index onward qualifies.
Binary search finds that first index, and the count is m - index.

### Trace

```
spells = [5,1,3], potions sorted = [1,2,3,4,5], success = 7

spell 5: limit = ceil(7/5) = 2 → first potion >= 2 is at index 1 → count = 5 - 1 = 4
spell 1: limit = ceil(7/1) = 7 → no potion >= 7, left = 5 → count = 5 - 5 = 0
spell 3: limit = ceil(7/3) = 3 → first potion >= 3 is at index 2 → count = 5 - 2 = 3

return [4,0,3]
```

### Details

- right starts at m, not m - 1, so "no potion qualifies" is a valid result.
- The loop is left < right, and right = mid keeps mid as a possible answer.

### Comparison

| Approach             | Time             | Space      |
| -------------------- | ---------------- | ---------- |
| Brute force          | O(n × m)         | O(n × m)   |
| Sort + binary search | O((n + m) log m) | O(1) extra |

### Key Learning

I never needed the products, only a count per spell.
Rearranging the inequality turned a matrix problem into a search problem.

---

**Streak: 126/184** 🔥
