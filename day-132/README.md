# Day 132/184 - November 9, 2026

## LeetCode 75: Letter Combinations of a Phone Number

### Problem

Each digit from 2 to 9 maps to letters like on a phone keypad. Return all letter combinations a string of digits could represent.

- Input: digits = "23"
- Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
- Edge case: digits = "" returns []

### Solution: Backtracking

```typescript
function letterCombinations(digits: string): string[] {
  if (digits.length === 0) {
    return [];
  }

  const map: Record<string, string> = {
    2: "abc",
    3: "def",
    4: "ghi",
    5: "jkl",
    6: "mno",
    7: "pqrs",
    8: "tuv",
    9: "wxyz",
  };

  const result: string[] = [];

  function backtrack(index: number, current: string): void {
    if (index === digits.length) {
      result.push(current);
      return;
    }

    for (const letter of map[digits[index]]) {
      backtrack(index + 1, current + letter);
    }
  }

  backtrack(0, "");
  return result;
}
```

### The Idea

The problem is a tree. Each level is one digit, and each branch is one letter of that digit.

```
                    ""
          /          |          \
         a           b           c        ← letters of 2
      / | \       / | \       / | \
    ad ae af    bd be bf    cd ce cf      ← letters of 3
```

Every path from the top to the bottom is one combination.

### What Each Part Does

- index: which digit I'm on
- current: the combination built so far
- Base case: index === digits.length means every digit has a letter, so save current
- The loop tries each letter of the current digit
- index + 1 moves to the next digit

### Trace

```
digits = "23"

backtrack(0, ""):
  'a' → backtrack(1, "a"): push "ad", "ae", "af"
  'b' → backtrack(1, "b"): push "bd", "be", "bf"
  'c' → backtrack(1, "c"): push "cd", "ce", "cf"
```

### First Attempt: What Was Missing

- No return after result.push(current), so the function kept going past the base case
- No loop over the letters of the current digit
- backtrack(0, "") was never called
- return result was missing
- Typo in the function name
- Missing types, so result was inferred as any[]

### Details

- Strings are immutable, so current + letter makes a new string each call and there is nothing to undo.
- If building with an array, push before the recursive call and pop after it. That undo step is the "back" in backtracking.

### Complexity

- Time: O(4ⁿ × n), each digit has at most 4 letters and building each string costs up to n
- Space: O(n) recursion depth, excluding the output

### Key Learning

Backtracking is recursion over a tree of choices.
Set the base case, loop over the choices, recurse one level deeper.

---

**Streak: 132/184** 🔥
