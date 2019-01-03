
# Backtracking/Exhaustive search


## Paradigm:

1. Same as recursion : Manually do the first step
2. Check if we have reached a valid result
    - If yes, print/store result
    - Else call recursive function for the remaining problem
3. **This is new**: Undo the changes made in step 1 after performing recursion

## When to use this:

Use backtracking/exhaustive search when you either have to go back or when the problem may have more than 1 result and you want all of them.

## Problems:

1. N Queens (Leetcode 51)
2. Permutations of a given string (Leetcode 46)
