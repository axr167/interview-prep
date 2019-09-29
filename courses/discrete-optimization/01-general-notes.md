

## Optimization Problems

There are multiple ways to solve optimization problems

- Dynamic Programming
- Constraint Programming
- Local Search
- Mixed Integer Programming

Generally speaking techniques like DP, CP and MIP give high quality solutions and are guaranteed to find the correct output if you give them enough time. Howrver they do not scale well. Local Search is the opposite - it scales really well however it may not give the correct solution. Hybrid is basically the holy grail of optimization that combines the two.

---------------------------------------------

### Dynamic Programming

- Look at brute force solution (permutations of all the possible ways to solve the problem)
- Model the problem as follows:
  - Define the decision variables (in knapsack is iten going to be picked or not, in graph coloring is the country goign to be red or blue, in n queens for row 1 the queen will be placed in colum 1 or 2 or ... n)
  - Define an objective function - what to maximize with respect to deicision variable
  - Define the constraints with respect to decision variable
  
## Branch and bound

- Model the problem
- Come up with optimistic value - what is the best possible value we can achieve if a given constraint did not exist.
  - This is the bound
- Expand the search tree normally making changes to optimistic value for each node as we expand.
- Keep track of the best value achieved so far.
  - If the optimistic value of a node is less than the actual value of the best node found so far, prune

## Constraint Programming

- Good for problems like n-queens, graph coloring etc where we have to find any feasible solution as opposed to maximizing or minimizing a value. But since it can find all feasible solutions given enough time it can find the optimal feasible solution.
- Unlike branch and bound this is branch and prune. We make a random choice and prune based on that choice.
  
