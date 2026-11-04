# Day 127/184 - November 4, 2026

## LeetCode 75: Find Peak Element

### Problem

A peak is an element strictly greater than its neighbours. Return the index of any peak in O(log n) time.

- Input: nums = [1,2,1,3,5,6,4]
- Output: 5 (value 6)
- Elements outside the array count as negative infinity.

### First Approach: Linear Scan (Wrong)

```typescript
function findPeakElement(nums: number[]): number {
  for (let i = 1; i < nums.length - 1; i++) {
    if (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
      return i;
    }
  }
}
```

Problems:

- The loop skips the first and last elements, so [1,2] and [1] fail
- Nothing is returned if no peak is found
- It is O(n), but the problem requires O(log n)

### Second Approach: Binary Search

```typescript
function findPeakElement(nums: number[]): number {
  let left = 0;
  let right = nums.length - 1;

  while (left < right) {
    const mid = Math.floor((left + right) / 2);

    if (nums[mid] < nums[mid + 1]) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }

  return left;
}
```

### The Idea

Binary search doesn't need a sorted array. It needs a way to know which half to discard.
Comparing nums[mid] with nums[mid + 1] gives that direction:

- nums[mid] < nums[mid + 1]: going uphill, a peak exists on the right, so left = mid + 1
- Otherwise: going downhill, mid may be the peak, so right = mid

A peak is always guaranteed on the uphill side because the end of the array counts as a peak against its imaginary negative infinity neighbour.

### Trace

```
nums = [1,2,1,3,5,6,4], left = 0, right = 6

mid = 3: nums[3] = 3 < nums[4] = 5 → uphill → left = 4
mid = 5: nums[5] = 6 < nums[6] = 4? No → downhill → right = 5
mid = 4: nums[4] = 5 < nums[5] = 6 → uphill → left = 5

left === right === 5 → return 5
```

### Details

- Loop on left < right, since this converges on one index instead of searching for a target
- right = mid, not mid - 1, because mid could be the answer
- mid + 1 is always in bounds because mid < right

### Comparison

| Approach      | Time     | Space |
| ------------- | -------- | ----- |
| Linear scan   | O(n)     | O(1)  |
| Binary search | O(log n) | O(1)  |

### Key Learning

Binary search works whenever one comparison lets you discard half the search space with certainty. The array doesn't have to be sorted.

---

**Streak: 127/184** 🔥
