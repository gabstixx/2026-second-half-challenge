# Day 129/184 - November 6, 2026

## LeetCode 75: Koko Eating Bananas

### Problem

Koko has piles of bananas and h hours. Each hour she picks one pile and eats k bananas from it. If the pile has fewer than k, she finishes it and does not start another pile that hour.
Find the minimum k that lets her finish within h hours.

- Input: piles = [3,6,7,11], h = 8
- Output: 4

### Solution: Binary Search on the Answer

```typescript
function hoursNeeded(piles: number[], k: number): number {
  let hours = 0;

  for (const pile of piles) {
    hours += Math.ceil(pile / k);
  }

  return hours;
}

function minEatingSpeed(piles: number[], h: number): number {
  let left = 1;
  let right = Math.max(...piles);

  while (left < right) {
    let mid = Math.floor((left + right) / 2);

    if (hoursNeeded(piles, mid) <= h) {
      right = mid;
    } else {
      left = mid + 1;
    }
  }

  return left;
}
```

### The Idea

The search is over the speed k, not over the array.
Slower speeds fail and faster speeds work, with one boundary between them:

```
k:      1  2  3  4  5  6  7
works:  ✗  ✗  ✗  ✓  ✓  ✓  ✓
                 ↑
          smallest working k
```

Binary search finds that boundary.

### Search Range

- left = 1, since she can't eat 0 bananas per hour
- right = max(piles), since eating the biggest pile in one hour is always enough and a higher speed gives no benefit

### The Check

Hours needed at speed k = sum of ceil(pile / k) over all piles.
ceil is needed because she can't start a new pile in the same hour, so a pile of 7 at speed 4 takes 2 hours (4, then 3).

### Trace

```
piles = [3,6,7,11], h = 8, left = 1, right = 11

mid = 6: hours = 1+1+2+2 = 6 <= 8 → right = 6
mid = 3: hours = 1+2+3+4 = 10 > 8 → left = 4
mid = 5: hours = 1+2+2+3 = 8 <= 8 → right = 5
mid = 4: hours = 1+2+2+3 = 8 <= 8 → right = 4

left === right === 4 → return 4
```

### Details

- right = mid when mid works, not mid - 1, because mid could be the smallest working speed
- Loop on left < right, since this converges on one value
- Math.ceil(pile / k) is the key line, plain division would undercount
- hoursNeeded takes piles as a parameter because it is a separate function

### Complexity

- Time: O(n log m), where m is max(piles). Each check is O(n) and there are about log m checks.
- Space: O(1)

### Key Learning

Binary search also works on the answer, not just on arrays.
Any problem with a fail-then-pass pattern across possible answers can use it.

---

**Streak: 129/184** 🔥
