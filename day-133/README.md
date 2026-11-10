# Day 133/184 — Combination Sum III

**Problem:** LeetCode 216 — Combination Sum III

## Problem Summary

We are given two integers, `k` and `n`.

The task is to find all valid combinations of `k` distinct numbers from `1` to `9` whose sum equals `n`.

Each number can only be used once in a combination.

## Understanding the Approach

The problem uses **Backtracking** to explore possible combinations.

The general idea is:

1. Start with an empty combination.
2. Choose a number from `1` to `9`.
3. Add it to the current combination.
4. Continue choosing numbers, making sure we don't reuse a number.
5. Backtrack when the current path cannot produce a valid combination.

### Important Conditions

- If the combination contains exactly `k` numbers and their sum equals `n`, add it to the result.
- If the combination already contains `k` numbers but the sum isn't `n`, stop exploring that path.
- If the current sum exceeds `n`, there is no need to continue that path.
- Use a starting index to ensure numbers are selected in increasing order and are not repeated.

## Key Insight

Backtracking allows us to explore different combinations while undoing previous choices when we need to try another possibility.

For this problem, the starting index is particularly important because it prevents us from selecting the same number more than once and avoids generating duplicate combinations.

## Today's Takeaway

I'm getting more familiar with how backtracking works through actual problems. The important part is understanding how to make choices, set stopping conditions, and know when to go back and explore another path.

**Streak: 133/184**
