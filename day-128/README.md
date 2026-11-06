# Day 128/184 — Koko Eating Bananas

**Problem:** LeetCode 875 — Koko Eating Bananas

## Today's Focus

Today was a bit stressful. I spent time trying to understand the problem and how binary search applies to it.

## Problem Summary

Koko has several piles of bananas and needs to finish eating all of them within `h` hours.

She chooses an eating speed of `k` bananas per hour.

- In each hour, she chooses one pile to eat from.
- If the pile has fewer than `k` bananas, she finishes that pile but cannot use the remaining time to eat from another pile.
- She wants to finish all the bananas within the available hours.

The goal is to find the **minimum eating speed** that allows her to finish on time.

## Understanding the Approach

The key is to apply binary search to the possible eating speeds rather than searching through the banana piles themselves.

We know:

- The minimum possible speed is `1`.
- The maximum necessary speed is the size of the largest pile.

For any proposed speed `k`, we calculate the hours needed for each pile:

`Math.ceil(pile / k)`

We add these hours together to get the total time required.

Then:

- If the total hours are greater than `h`, Koko is eating too slowly. We need a higher speed.
- If the total hours are less than or equal to `h`, the speed is sufficient. We try a lower speed to find the minimum possible one.

This is a form of **binary search on the answer**.

## Key Lesson

Binary search isn't limited to finding an element in a sorted array. It can also help us find the minimum or maximum value that satisfies a condition.

Today's challenge was understanding how to identify that condition and use it to narrow down the possible answers.

**Streak: 128/184**
