
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
