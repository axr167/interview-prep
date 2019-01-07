
# Prerequisites

The following are prerequisites to understand this text:

1. You should be able to come up with brute force solutions to problems
2. You must have a good understanding of recursion and recurrence relations
3. If you think you do not have a firm understanding of the above, please read my notes on recursion which is also in this repository.

# Overview of Dynamic Programming (DP)

Sometimes when solving a problem using recursion, the same problems are computed over and over. This increases the time complexity.

Take for example the fibonacci sequence. Here the recurrence relation is:
- f(n) = f(n-1) + f(n-2)

So suppose we have n = 100

- We have 2 subproblems for f(n) these are f(n-1) and f(n-2)
- We compute f(99) (Subproblem 1) and f(98) (Subproblem 2)
- When computing Subproblem 1 we compute:
    - f(98) and f(97) and this continues for the 2 other subproblems generated
- We then compute Subproblem 2 f(98)
    - However notice that we have already computed f(98) in Subproblem 1.
    - But we sitll compute it again anyway because we have to do it
    - If we had stored f(98) from subproblem 1 somewhere we could just avoid computing Subproblem 2 and use the stored value instead

That is the bacic idea of DP. Whenever we encounter subproblems that overlap as in the case of Subproblems 1 and 2, instead of computing the Subproblems all over again, we store each computation in a cache like an array or a matrix. Then if we ever need to compute something a second time, we simply get the value from the cache greatly decreasing the time complexity.

This is Dynamic Programming.

# When to use DP?

DP should be used if:

1. The problem is split into 2 or more subproblems AND
2. The subproblems overlap

**So how to determine if the subproblems overlap?**

This can be determined by looking at the recurrence relation. The recurrence relation depends on 1 or more input variables that are changing in some way.
- In the relation f(n) = f(n-1) + f(n-2) the variable n is changing across the 2 subproblems.

The way I visialize this is as follows:

Let us consider the fibonacci sequence where problems overlap

- Let 'n' be a very large number say a million or a billion. The numbers we care about is n-1 and n-2 since those are in the subproblems.
- Draw a line for Subproblem 1 that goes from n-1 to the base case (1). Draw another line for Subproblem 2 that goes from n-2 to the base case (1).
- If the 2 lines overlap, the subproblems overlap.
- An illustration for the above is shown below:

![overlap](https://i.imgur.com/DJTyVsV.png)

Now let us consider something like Binary search where problems DO NOT overlap

- The recurrence relation is: f([0...n]) = f([0 ... n/2]) | f([n/2+1 ... n]) where n is the size of the array
- Here the 2 subproblems do not overlap no matter how large n becomes. 
- This is illustrated below

![no-overlap](https://i.imgur.com/6PDI4ps.png)

# How to determine the size of the cache?

The cache size can be determined by looking at changing input variables in the recurrence relation. If there exists 1 variable n that changes in the relation, then the size of cache is at most 'n' if there exist 2 variables (m, n) the size of the cache is at most mxn and so on.

**[NOTE]: The size of the cache can be reduced in many cases. This is just the upper limit**

Let us consider some examples. Consider the recurrence relations below:

1. f(n) = f(n-1) + f(n-2): Here the maximum size of the cache is 'n'
2. f(i, j) = max(f(i+1, j), f(i, j+1): Here the maxumum size of the cache is ixj.

# The two types of DP

There are 2 ways by which we can solve a DP problem. The first is Top Down and the second is Bottom Up. They are described below.

## 1. Top-Down DP

This is just recursion with caching. The paradigm is:

- If base case, store in cache and return the answer
- Else:
    - Check cache for answer. If answer exists return it
    - Otherwise recurse and store result in cache
    - Return result from cache

## 2. Bottom-Up DP

Here instead of using recursion, we go bottom up using for-loops. The direction of the loops can be estimated by looking at the recurrence relation.

Consider the following recurrence relation: f(n) = f(n+1) + f(n+2)

- There is 1 changing input variable n so there exists 1 for loop.
- The value of f(n) depends on n+1 and n+2
    - This implies that in the cache, cache[n+1] and cache[n+2] are filled before cache[n] is filled
    - So the loop travels backwards from n to 0
 
Bottom up DP has a few advantages over top down DP

- Since there is no recursion involved there are no overheads related to the recursion stack
- It is easy to optimize the size of the cache simply by observing how the cache is filled (this will be discussed in the problems)

However on the other hand it can be a bit tricky and harder to implement when compared to Top-Down DP.


# Example Problems

All the examples will have both the top down and the bottom up solutions. Each example will add someting new so I highly recommend reading through all of them. The examples are:

1. nth Fibonacci Number: Introduces the basic concepts of DP and how space can be optimized through bottom up DP
2. Longest Increasing Path in a Matrix (Leetcode 329): A slightly more compex problem
3. Bytelandian gold coins (Codechef): Shows a case where Top-Down DP is better than Bottom-Up DP and how recurrence relation of Bottom-Up DP can be changed in order to make it as good as the Top-Down version.
4.

# 1. Fibonacci

import java.util.*;

public class Solution{
    
    /*
    
        Converting Brute force to Top Down DP 
            - Do base case as usual
            - In the recursive step:
                - If cache has the answer then return it
                - Else recurse and store answer in cache
                - Return value in cache
    
    */
    
    
    /* BRUTE FORCE */
    private static int fib(int n) {
        // BASE CASE
        if(n==0 || n==1)
            return 1;
        // RECURSIVE STEP
        else 
            return fib(n-1) + fib(n-2);
    }
    
    /* MEMOIZATION */
    private static int mem_fib(int n, int[] dp) {
        // BASE CASE
        if(n==0 || n==1)
            return 1;
        // RECURSIVE STEP
        else {
            // If cache has answer return it else recurse and store answer in cache
            if(dp[n]!= -1)
                return dp[n];
            else {
                dp[n] = fib(n-1) + fib(n-2);
            }
            return dp[n];
        }
    }
    
    
    
    public static void main(String []args){
        
        int[] dp = new int[20];
        Arrays.fill(dp, -1);
        
        for(int i = 0; i<20; i++) {
            System.out.println("i, fib, memfib: " + i + " " + fib(i) + " " + mem_fib(i, dp) );
        }
        
        
    }
}


# 2. 0/1 Knapsack Problem

    import java.util.*;

    public class Solution{


        // If current = w.length return 0 (BASE CASE)
        // if W > w
        //      f(W, c) = max(f(W, c+1), v[i]+f(W-w[c], c+1))
        // Else f(W, c) = f(W, c+1)

       private static int mem_knap(int[] w, int[] v, int W, int i, int[][] dp) {
            if(i == w.length)
                return 0;
            else {
                if(dp[i][W] != -1)
                    return dp[i][W];
                else {
                    if(W >= w[i] )
                        dp[i][W] = Math.max( mem_knap(w,v,W,i+1,dp), 
                                v[i] + mem_knap(w,v,W-w[i],i+1,dp) );
                    else
                        dp[i][W] = mem_knap(w,v,W,i+1,dp);
                }
                System.out.println(Arrays.deepToString(dp));
                return dp[i][W];
            }
       }

        public static void main(String []args){
            int[] w = {4,1,2,3,2,2}; // item weights
            int[] v = {5,8,4,0,5,3}; // item values

            int W = 3; // Total knapsack capacity

            int[][] dp = new int[w.length][W+1];
            for(int[] row : dp)
                Arrays.fill(row, -1);


            System.out.println(mem_knap(w,v,W,0, dp));

        }
    }
