# Day 125/184 — Guess Number Higher or Lower

**Problem:** LeetCode 374 — Guess Number Higher or Lower

## Problem Summary

We are given a number `n` and need to find a hidden number between `1` and `n` using the provided `guess()` API.

The API returns:

- `-1` if our guess is higher than the picked number.
- `1` if our guess is lower than the picked number.
- `0` if our guess is correct.

## My Initial Approach

My first thought was to loop through the numbers from `1` to `n`, guessing each number until I found the answer.

This resulted in TLE (Time Limit Exceeded).

I realized that I was checking every possible number one after another. That was brute force, not binary search.

## Optimized Approach: Binary Search

Binary search works by repeatedly dividing the search space into two halves.

I used two boundaries:

- `i = 1`: The left boundary.
- `n`: The right boundary.

For each iteration:

1. Calculate the middle value using `Math.floor((i + n) / 2)`.
2. Pass the middle value to the `guess()` API.
3. If the result is `0`, return the middle value.
4. If the result is `1`, the picked number is higher, so update `i = mid + 1`.
5. If the result is `-1`, the picked number is lower, so update `n = mid - 1`.

This continues until the correct number is found.

## Implementation

```typescript
function guessNumber(n: number): number {
  let i = 1;

  while (i <= n) {
    const mid = Math.floor((i + n) / 2);
    const result = guess(mid);

    if (result === 0) {
      return mid;
    } else if (result === 1) {
      i = mid + 1;
    } else if (result === -1) {
      n = mid - 1;
    }
  }

  return -1;
}
```

## Complexity Analysis

- **Time:** `O(log n)` — each iteration eliminates approximately half of the remaining search space.
- **Space:** `O(1)` — only a few variables are used.

## Key Lesson

The biggest takeaway was understanding that binary search is not just about finding the middle element. It's about correctly adjusting the search boundaries after every comparison.

I started with brute force, encountered TLE, and eventually understood how to use binary search to make the solution more efficient.

**Streak: 125/184**
