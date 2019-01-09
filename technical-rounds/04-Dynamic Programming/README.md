
# Prerequisites

The following are prerequisites to understand this text:

1. You should be able to come up with brute force solutions to problems
2. You must have a good understanding of recursion and recurrence relations
3. If you think you do not have a firm understanding of the above, please read my notes on recursion which is also in this repository.

# Overview of Dynamic Programming (DP)

When multiple subproblems in a recurrence overlap, recursion or brute force computes certain precomputed values over and over again. To avoid this, we save already computed values into a cache where precomputed values can be retreived in constant time (such as an array or a hashmap).

The technique of saving computed values into a cache so we do not have to compute them again is known as DP. It should be used if there are multiple subproblems AND the subproblems overlap

**Overlapping Subproblems**

The following subproblems overlap:

- f(i, j) = f(i+1, j) + f(i, j-1)
- f(x) = f(x+2) - f(x+3)
- f(a, b) = max( f(a, b), f(a, b-array[i])

The following subproblems do not overalp:

- f(a) = f(a-1)
- f(a[0 ... n]) = f(a[0 ... n/2]) & f(a[n/2 ... n])

**How to determine the size of the cache?**

Look at all the variables (v1, v2, ... vn) in the recurrence that are involved in conditional statements. The size of the cache is: (v1 x v2 x ... vn)

Example 1 (Fibonacci):

	f(n) = 1 if n == 0 or 1
	f(n) = f(n-1) + f(n-2) otherwise
Here 1 variable n is in the conditional so max cache size is in the order of: n

Example 2 (Knapsack):

	f(W, i, v) = v // i >= n
	f(W, i, v) = f(W, i+1, v)  // w[i] > W
	f(W, i, v) = max( f(W, i+1, v), f(W-w[i], i+1, v+v[i]) ) // otherwise
Here 2 variables i, W are in the conditional so max cache size is in the order of: i * W


**NOTE: The size of the cache can be reduced in many cases. This is just the upper limit**

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
2. Min path sum (Leetcode): A slightly more compex problem
3. Bytelandian gold coins (Codechef): Shows a case where Top-Down DP is better than Bottom-Up DP and how recurrence relation of Bottom-Up DP can be changed in order to make it as good as the Top-Down version.
4. Longest Increasing Path in a Matrix: Shows a case where it is extremely difficult to get a Bottom Up solution which is equivalent to the corresponding top down solution (to do that we must model it as a graph and use topological sort)

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

# 2. Min path sum

Recurrence relation:

    // f(i,j) = a[i][j] if i == m j == n
    // f(i,j) = a[i][j] + f(i, j+1) if i == m
    // f(i,j) = a[i][j] + f(i+1, j) if j == n
    // f(i,j) = a[i][j] + min(f(i, j+1), f(i+1, j)) otherwise

Recursion:

    private int f(int[][] a, int i, int j) {
        if(i == a.length-1 && j == a[0].length-1)
            return a[i][j];
        else if(i == a.length-1)
            return a[i][j] + f(a, i, j+1);
        else if(j == a[0].length-1)
            return a[i][j] + f(a, i+1, j);
        else
            return a[i][j] + Math.min( f(a, i, j+1), f(a, i+1, j) );
    }

Top-Down:
    
    private int f(int[][] a, int i, int j, int[][] dp) {
        if(i == a.length-1 && j == a[0].length-1) {
            if(dp[i][j] == 0)
                dp[i][j] = a[i][j];
        } else if (i == a.length-1) {
            if(dp[i][j] == 0)
                dp[i][j] = a[i][j] + f(a, i, j+1, dp);
        } else if (j == a[0].length-1) {
            if(dp[i][j] == 0)
                dp[i][j] = a[i][j] + f(a, i+1, j, dp);
        } else {
            if(dp[i][j] == 0)
                dp[i][j] = a[i][j] + Math.min(f(a, i, j+1, dp), f(a, i+1, j, dp));
        }
        return dp[i][j];
    }
    

Bottom-Up

    private int f(int[][] a) {
        int[][] dp = new int[a.length][a[0].length];
        for(int i = a.length-1; i>=0; i--) {
            for(int j = a[0].length-1; j >=0; j--) {
                if(i==a.length-1 && j == a[0].length-1)
                    dp[i][j] = a[i][j];
                else if(i==a.length-1)
                    dp[i][j] = a[i][j] + dp[i][j+1];
                else if(j ==a[0].length-1)
                    dp[i][j] = a[i][j] + dp[i+1][j];
                else
                    dp[i][j] = a[i][j] + Math.min(dp[i][j+1], dp[i+1][j]);
            }
        }
        return dp[0][0];
    }
    

Bottom-Up Space Optimized

    private int f(int[][] a) {
        int[] row1 = new int[a[0].length];
        int[] row2 = new int[a[0].length];
        for(int i = a.length-1; i>=0; i--) {
            for(int j = a[0].length-1; j >=0; j--) {
                if(i == a.length-1 && j == a[0].length-1)
                    row1[j] = a[i][j];
                else if(i == a.length-1)
                    row1[j] = a[i][j] + row1[j+1];
                else if(j == a[0].length-1)
                    row1[j] = a[i][j]+ row2[j];
                else
                    row1[j] = a[i][j] + Math.min(row1[j+1], row2[j]);
            }
            row2 = row1;
        }
        return row1[0];
    }

# 3. 0/1 Knapsack Problem

We define items as follows:

    public class KnapsackItems {
        public int[] wt;
        public int[] val;
        public KnapsackItems(int[]x, int[] y) {
            wt = new int[x.length];
            val = new int[y.length];
            for(int i = 0; i<x.length; i++) {
                wt[i] = x[i];
                val[i] = y[i];
            }
        }
    }

Main program:

    public static void main(String[] args) {
		int[] w = {4,1,2,3,2,2}; // item weights
		int[] v = {5,8,4,0,5,3}; // item values
		KnapsackItems I = new KnapsackItems(w, v);
		Solution s = new Solution();
		System.out.println(s.solve(I, 3) );
	}
