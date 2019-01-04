
# Backtracking/Exhaustive search (In progress)


## Paradigm:

- Similar to recursion but with a minor difference: Base case is result so instead of returning, save/print result
- In induction step after performing recursion undo changes to reset problem to original state

Paradigm is:

- If base case Save/print output
- Else do induction step - This is done a finite 'n' number of times which is equal to the search space
    - Do first step reducing problem size
    - Recurse for subproblem
    - Undo all changes
      

## When to use this:

Use backtracking/exhaustive search when you either have to go back or when the problem may have more than 1 result and you want all of them.

## Problems:

1. N Queens (Leetcode 51)
2. Permutations of a given string (Leetcode 46)
3. Word Search (Leetcode 79)
4. Sudoku solver (Leetcode 37)
5. Combination Sum (Leetcode 39)
