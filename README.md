# Interview Prep
Various notes I have compiled while preparing for interviews

## Resources:
   - Java collections cheatsheet: Overview of methods for java.util.Collections. Some content taken from [here](https://courses.cs.washington.edu/courses/cse143/17su/exams/final/cheat_sheet.pdf). (WORK IN PROGRESS)
   - Regex Cheatsheet: Overview of regular expressions
   - Coming soon: Design patterns cheatsheet; Spring overview

# Problem Solving Paradigms

**1. Simplify and Delegate: Basic recursion/induction**
  - If given instance of the problem can be solved directly, solve it (BASE CASE)
  - Otherwise do the following: (INDUCTION STEP)
    - Assume any smaller instance of the problem can be solved by your function
    - Perform a single step to reduce problem size
    - Call same function for smaller problem. We know this can be solved due to our assumption (see strong mathematical induction)
    
**2. Exhaustive Search: Need to traverse through entire search space - can use either iteration of backtracking. Iteration is straightforward. Backtracking described below.**
  - If base case store/print/return
  - Else do the following:
    - Perform single step to reduce problem size
    - Call recursive function
    - Undo changes made and continue

**3. Greedy: If locally optimal choice is solution for problem.**
  - Needs optimal substructure and greedy property (we do not look back to previous picks)

**4. Dynamic Programming: Use cache/memory to quickly access data as opposed to doing recursion again**
