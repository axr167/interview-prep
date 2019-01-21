
# Prerequisites

The following are prerequisites to understand this text:

1. You should be able to come up with brute force solutions to problems
2. You must have a good understanding of recursion and recurrence relations
3. If you think you do not have a firm understanding of the above, please read my notes on recursion which is also in this repository.

# Overview of Dynamic Programming (DP)

When multiple subproblems in a recurrence overlap, recursion or brute force computes certain precomputed values over and over again. To avoid this, we save already computed values into a cache where precomputed values can be retreived in constant time (such as an array or a hashmap).

The technique of saving computed values into a cache so we do not have to compute them again is known as DP. It should be used if there are multiple subproblems AND the subproblems overlap

## Overlapping Subproblems

The following subproblems overlap:

	1. f(i, j) = f(i+1, j) + f(i, j-1)
	2. f(x) = f(x+2) - f(x+3)
	3. f(a, b) = max( f(a, b), f(a, b-array[i])

The following subproblems do not overalp:

	1. f(a) = f(a-1)
	2. f(a[0 ... n]) = f(a[0 ... n/2]) & f(a[n/2 ... n])

## How to determine the size of the cache?

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

**Tips on setting up the cache**

- If your recurrence relation is in the form: f(i) = max(f(i+1), f(i)):
	- It is always safe to make your array of size [i+1] instead of [i] to accomodate for base cases.

## The two methods to solve DP

There are 2 methods for solving a DP problem. The first is Top-Down and the second is Bottom-Up. They are described below.

**1. Top-Down DP**

This is just recursion with caching. The paradigm is:

- If base case, store in cache and return the answer
- Else:
    - Check cache for answer. If answer exists return it
    - Otherwise recurse and store result in cache
    - Return result from cache

**2. Bottom-Up DP**

Here instead of using recursion, we go bottom up using for-loops. The direction of the loops can be estimated by looking at the recurrence relation.

Consider the following recurrence relation: f(n) = f(n+1) + f(n+2)

- There is 1 changing input variable n so there exists 1 for loop.
- The value of f(n) depends on n+1 and n+2
    - This implies that in the cache, cache[n+1] and cache[n+2] are filled before cache[n] is filled
    - So the loop travels backwards from n to 0
 
## Optimizing Space complexity using Bottom-up DP

It is possible to optimize space complexity when using Bottom-Up DP. This is possible when we have a subproblem in the form f(n) = f(n+c) or f(n) = f(n-c) where 'c' is a constant. 

In this case the space complexity can be reduced by upto one dimension in the order of n. Instead of n, instead of 'n', the space can be reduced to just 'c'.

So the expressions: 

- f(n) = f(n-1) + f(n-2) can be reduced from n to just 2.
- f(i,j) = max(f(i+1, j) + f(i, j+1)) can be reduced from i * j to just j
- f(i, j) = max(f(i+1,j), f(i, j-a[n])) can be reduced from i * j to just j

Let us take example 2 - here the values of row i depends only on the values of row i+1. It does not depend on values of rows i+2, i+3 etc so instead of maintaining space for all the rows, we can just discard all the useless ones and save space only for what we need.

**Note: Order of the loops when performing space optimization matters. The loop that iterates on the dimension to be reduced MUST be the outermost loop.**

## Top-Down vs Bottom-Up

Some comparisons between Top-Down and Bottom-Up are:

1. Bottom-up can be space optimized unlike top-down
2. Bottom-up does not use recursion so there is no extra overhead for recursive calls
3. Top-down is easier to implement as it is essentially the same as recursion. Implementing Bottom-up is a bit trickier.
4. Top-down can sometimes have better time complexity because top down only evaluates subproblems that it needs however since we perform iteration in Bottom-up, every state (even ones that we do not need) are evaluated. This is true for type 3 problems below.

## Three types of DP problems

I have come up with 3 distinct types of DP problems based on what their recurrence relations look like.

### Type 1: When the recurrence has simple addition/subtraction with a constant term

For example:

	f(n) = f(n-1) + f(n-2)
	f(i,j) = max(f(i-1,j), f(i,j+1))

This is the simplest form of DP problem. 

To solve Type 1 problems solve using bottom up

### Type 2: When the recurrence has division

For example:

	f(n) = max(n, f(n/2) + f(n/3))

Here doing Bottom-Up DP will have a space and time complexity of 'n' which is not good as all states are computed. Since Top-Down only computes the states that is necessary, it will be significantly faster. 

For example if n=1000, Bottom-Up would compute states where n ranges from 500 to 1000 which is unnecessary as it will never be used.

Here we must convert the recurrence as follows:

	f(n) = max(n, f(n/2) + f(n/3))
	=> f( n/(2^0 * 3^0) ) = max( (n /(2^0 * 3^0)), f( n/(2^1 * 3^0) ) + f( n/(2^0 * 3^1) ) )
	=> f( n/(2^i * 3^j) ) = max( (n /(2^i * 3^j)), f( n/(2^i+1 * 3^j) ) + f( n/(2^i * 3^j+1) ) )
	=> f(i,j) = max( (n /(2^i * 3^j), f(i+1,j) + f(i, j+1))

Notice the term: (n /(2^i * 3^j)

	We know max possible values of i, j are: n = 2^i and n = 3^j
	Hence max values of i, j are: log_2(n) and log_3(n) [add 1 to max value them just to be safe]
	
Thus the problem is reduced from size n to just (log(n))^2 which is much better.

To solve Type 2 DP problems optimize the recurrance and solve using Bottom-Up DP.

### Type 3: When the recurrence has a variable term

For example:

	f(i, j) = max(f(i+1, j), f(i, j-arr[i])) where arr is some array containing values

Here Top-Down will have better time complexity as some states are useless and Bottom-Up evaluates them anyway however on the other hand with Bottom-Up there is potential for optimizing the space complexity.

To solve Type 3 problems do either depending on what you need.


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

    // f(i,j) = a[i][j] if i == m; j == n
    // f(i,j) = a[i][j] + f(i, j+1) if i == m
    // f(i,j) = a[i][j] + f(i+1, j) if j == n
    // f(i,j) = a[i][j] + min(f(i, j+1), f(i+1, j)) 

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
        int[][] dp = new int[a.length+1][a[0].length+1];
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
	
Recurrence relation:

	/* 
	 * RECURRENCE:
	 *  f(W, i, v) = v // i >= n
	 *  f(W, i, v) = f(W, i+1, v)  // w[i] > W
	 *  f(W, i, v) = max( f(W, i+1, v), f(W-w[i], i+1, v+v[i]) ) // otherwise
	 */

Recursive:

	private int f(KnapsackItems I, int W, int i, int v) {
		int n = I.wt.length;
		if(i == n)
			return v;
		if(I.wt[i] > W)
			return f(I, W, i+1, v);
		else
			return Math.max(f(I, W, i+1, v), f(I, W-I.wt[i], i+1, v+I.val[i]));
	}

Top-Down:

	private int f(KnapsackItems I, int W, int i, int v, int[][]dp) {
		int n = I.wt.length;
		if(dp[i][W] != 0)
			return dp[i][W];
		if(i == n) 
			dp[i][W] = v;
		else if(I.wt[i] > W)
			dp[i][W] = f(I, W, i+1, v, dp);
		else
			dp[i][W] = Math.max(f(I, W, i+1, v), f(I, W-I.wt[i], i+1, v+I.val[i]));
		return dp[i][W];
	}

Bottom-Up:

	private int f(KnapsackItems I, int W) {
		int n = I.wt.length;
		int[][] dp = new int[n+1][W+1];
		
		for(int i= n-1; i>=0; i--) {
			for(int j=0; j<W+1; j++) {
				if(i >= n)
					dp[i][j] = 0;
				else if(I.wt[i] > j)
					dp[i][j] = dp[i+1][j];
				else
					dp[i][j] = Math.max(dp[i+1][j], I.val[i]+dp[i+1][j-I.wt[i]]);
			}
		}
		return dp[0][W];
	}

Bottom-Up space optimized

	private int fs(KnapsackItems I, int W) {
		int n = I.wt.length;
		int[] row1 = new int[W+1];
		int[] row2 = new int[W+1];
		
		for(int i = n-1; i>=0; i--) {
			for(int j=0; j<W+1; j++) {
				if(i >=n)
					row1[j] = 0;
				else if(I.wt[i] > j)
					row1[j] = row2[j];
				else
					row1[j] = Math.max(row2[j], I.val[i]+row2[j-I.wt[i]]);
			}
			for(int j=0; j<W+1; j++)
				row2[j] = row1[j];
		}
		return row1[W];
	}

# 4. Longest Pallindromic substring


Bottom-Up

    private String solve(String s) {
        String[][] dp = new String[s.length()+1][s.length()+1];
        for(int i = s.length()-1; i >= 0; i--) {
            for(int j = 0; j<=s.length(); j++) {
                if(j<=i)
                    dp[i][j] = "";
                else {
                    if(isP(s.substring(i,j))) {
                        dp[i][j] = s.substring(i,j);
                    } else {
                        if(dp[i][j-1].length() > dp[i+1][j].length())
                            dp[i][j] = dp[i][j-1];
                        else
                            dp[i][j] = dp[i+1][j];
                    }
                }
            }
        }
        return dp[0][s.length()];
    }
    

Bottom-Up space optimized

    private String solve(String s) {
        String[] row1 = new String[s.length()+1];
        String[] row2 = new String[s.length()+1];
        for(int i = s.length()-1; i >= 0; i--) {
            for(int j = 0; j<=s.length(); j++) {
                if(i>=j)
                    row1[j] = "";
                else if (isP(s.substring(i,j)))
                    row1[j] = s.substring(i,j);
                else {
                    if(row1[j-1].length() > row2[j].length())
                        row1[j] = row1[j-1];
                    else
                        row1[j] = row2[j];
                }
            }
            for(int x = 0; x< row1.length; x++) {
                row2[x] = row1[x];
            }
        }
        return row1[s.length()];
    }

# Longest Increasing Subsequence

Recurrence:
        
        f(p, i) = 0 // if i = a.length
        f(p, i) = max(1+f(i, i+1), f(p, i+1)) // if(a[i] > a[p])
        f(p, i) = f(p, i+1) otherwise
	
let prev = a[j]
	
	f(prev, i) = 0 // if i = a.length
        f(prev, i) = max(1+f(a[i], i+1), f(prev, i+1)) // if(a[i] > prev)
        f(prev, i) = f(prev, i+1) otherwise
        
Recursion:

    private int f(int[] a, int p, int i) {
        if(i==a.length)
            return 0;
        if(a[i] > a[p])
            return Math.max(f(a, p, i+1), 1+f(a, i, i+1));
        else
            return f(a, p, i+1);
    }
    
Top-Down:

    private int fm(int[] a, int p, int i, int[][] dp) {
        if(i == a.length) {
            dp[p][i] = 0;
            return dp[p][i];
        }
        if(dp[p][i] != 0)
            return dp[p][i];
        if(a[i] > a[p])
            dp[p][i] = Math.max(fm(a, p, i+1, dp), 1+fm(a, i, i+1, dp));
        else
            dp[p][i] = fm(a, p, i+1, dp);
        return dp[p][i];
    }

Bottom-Up:

    private int fd(int[] a) {
        int[][] dp = new int[a.length+1][a.length+1];
        for(int p = a.length-1; p >=0; p--) {
            for(int i = a.length-1; i >=0; i--) {
                if(i >= a.length)
                    dp[p][i] = 0;
                else if(a[i] > a[p])
                    dp[p][i] = Math.max(dp[p][i+1], 1 + dp[i][i+1]);
                else
                    dp[p][i] = dp[p][i+1];
            }
        }
        return dp[0][0];
    }

Bottom-Up Space Optimized: Only outer loop can be optimized so make 'i' the outer loop and 'p' the inner loop

    private int fs(int[] a) {
        int[] col1 = new int[a.length+1];
        int[] col2 = new int[a.length+1];
        for(int i = a.length-1; i >=0; i--) {
            for(int p = a.length-1; p >=0; p--) {
                if(a[i] > a[p]) 
                    col1[p] = Math.max(col2[p], 1+col2[i]);
                else
                    col1[p] = col2[p];
            }
            for(int p=0; p< col1.length; p++)
                col2[p] = col1[p];
        }
        return col1[0];
    }


# Longest Common Subsequence

Recurrence relation:

    /*
        Recurrence:
            f(i, j) = 0 //if i=0 || j = 0
            f(i, j) = 1 + f(i+1, j+1) //if(s1[i] = s2[j])
            f(i, j) = max(f(i,j+1), f(i+1, j)) // otherwise
        
    */


Recursion:

    private int f(int[] a, int[] b, int i, int j) {
        if(i == a.length || j == b.length)
            return 0;
        else if(a[i] == b[j])
            return 1+f(a,b,i+1, j+1);
        else
            return Math.max(f(a, b, i, j+1), f(a, b, i+1, j));
    }

Top-Down:

    private int fm(int[] a, int[] b, int i, int j, int[][] dp) {
        if(i == a.length || j == b.length) {
            dp[i][j] = 0;
            return dp[i][j];
        }
        if(dp[i][j] != 0)
            return dp[i][j];
        if(a[i] == b[j])
            dp[i][j] = 1+fm(a,b,i+1, j+1, dp);
        else
            dp[i][j] = Math.max(fm(a, b, i, j+1, dp), fm(a, b, i+1, j, dp));
        return dp[i][j];
    }

Bottom-Up:


# Longest Common Subarray/Substring



