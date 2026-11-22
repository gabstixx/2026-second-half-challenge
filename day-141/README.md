# Day 141/184 — House Robber

**Problem:** LeetCode 198 — House Robber

## Problem Summary

A robber wants to collect the maximum amount of money from a row of houses.

Each house contains a certain amount of money, but the robber cannot rob two adjacent houses because doing so would trigger an alarm.

The goal is to find the maximum amount that can be collected without robbing neighbouring houses.

## Understanding the DP Approach

At each house, we have two choices:

1. **Rob the current house:** Add its money to the maximum amount collected up to two houses before it.
2. **Skip the current house:** Keep the maximum amount collected up to the previous house.

We compare both choices and take the larger result.

The recurrence is:

\[
dp[i] = \max(dp[i-1],\ nums[i] + dp[i-2])
\]

Where:

- `dp[i-1]` represents skipping the current house.
- `nums[i] + dp[i-2]` represents robbing the current house.

## Key Insight

The important part is understanding that the decision at each house depends on the best results from previous houses.

Instead of checking every possible combination of houses, Dynamic Programming allows us to reuse previously calculated results.

## Today's Takeaway

I'm learning to recognize how a problem can be broken down into smaller decisions and how those decisions contribute to the final answer.

Still working on understanding the logic before implementation.

**Streak: 141/184**
