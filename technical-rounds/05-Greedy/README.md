
# Overview

**Note: Future reading (not necessary) - 1. Matroids 2. Linear Programming Duality. It will help you understand how greedy algorithms are devised**

A greedy algorithm is one that builds on the solution by choosing best option at each step. Since greedy only gets the local optimum, this might not be the global optimum hence greedy algorithms do not always work. We must be able to prove that the greedy algorithm arrives at the most optimal answer for it to be correct.

The steps involved in greedy algorithms are as follows:

- Figure out optimum solution for first step given only the information available in the current state (Base case)
  - This is often done by ordering the input in some way.
- Build on this (using a loop) by identifying the next step until we get a solution. (Induction step)
  - Once you have chosen something and have a part of the solution you do not change it. You must build on it.

We can identify the steps to take by looking at the 'local improvement' at each step.

### Local improvement.

Suppose you have the solution at the first step. Ask the following question:

  - Is it possible to add 1 or more values by removing at most 1 value from the solution?
      - If you want to add something it should not affect the second value it should at most affect 1 value.
  - If yes what is the criteria?

Greedy is basically mathematical induction

- The first case is the base case
- Then we proceed to the solution following the same pattern as previous steps (induction step)

The local improvement/exchange trick can be used when you need a subset from a given set that matches certain conditions.

- Independent set from forest of trees: find max subset (subset from set) such that no 2 elements are connected (condition)
- Maximum intervals: Get max intervals from set of intervals (subset from set) such that no 2 intervals overlap (condition)
- Minimize the sum of set of jobs

The other thing to try is to sort the input in some way and try to wing it. Here the question is how to order the items.

- Minimum number of partitions for a set of intervals such that no 2 intervals overlap
- Fractional knapsack
