
# Problem: Solve Knapsack for a bunch of datasets

Need to return the following:

    Optimal_value     Are_we_sure_its_optimal

    0_1_Array_for_picked_items

Example

    19  1
    0   0   1   1
    
- This means 19 is the optimal value, 1 means that we are 100% sure 19 is optimal and not just a greedy approximation
- The bottom array means items 0,1 were not picked and items 2,3 were picked to get optimal value


# Approach 1: Dynamic Programming (not optimized for space)

**The recurrence is:**

    Let n be the nth item 
    let c be the remaining capacity in the knapsack

    f(n, c) = 0             // if n=0
    f(n, c) = f(n-1, c)     // if weight[n] > c
    f(n, c) = max(f(n-1, c), value[n] + f(n-1, c-weight[n]))  // if weight[n] <= c

**Logic to get the items is (for ks_4_0)**

1. The matrix dp[][] is as follows:

<pre>
      i\j   |    0       1       2       3       4       5       6       7       8       9       10      11    
    ========================================================================================================
            |
       0    |    0       0       0       0       0       0       0       0       0       0       0       0
            |
       1    |    0       0       0       0       8       8       8       8       8       8       8       8
            |
       2    |    0       0       0       0       8       10      10      10      10      18      18      18
            |
       3    |    0       0       0       0       8       10      10      10      15      18      18      18
            |
       4    |    0       0       0       4       8       10      10      12      15      18      18      19
</pre>

2. Start at bottom right corner dp[4][11]. Compare it with previous row dp[3][11]. Value 19 != 18. Hence **Item 4 was chosen**. If item is chosen go to dp[i-1][ j-weight[chosen_item] ] => dp[3][8]
3. Compare dp[3][8] with dp[2][8]. It is different so **Item 3 was chosen**. Go to dp[i-1][ j-weight[chosen_item] ] => dp[2][0]
4. Compare dp[2][0] with dp[1][0]. The values are the same so **Item 2 was not chosen**. Go to dp[i-1][j] => dp[1][0].
5. Compare dp[1][0] with dp[0][0]. The values are the same so **Item 1 was not chosen**. Go to dp[0][0]. This is the terminal state so we end.

-------------------------------------------

### DP solution with time and space O(n^2):


    private static void backtraceDP(int[][] dp, int[] weight, int[] taken) {
        int i = dp.length-1;
        int j = dp[0].length-1;

        while(i != 0 ) {
            if(dp[i][j] == dp[i-1][j]) {
                i = i-1;
            } else {
                taken[i-1] = 1;
                j = j-weight[i-1];
                i = i-1;
            }
        }
    }

    private static int knapsackDP(int[] v, int[] w, int k, int[] taken) {
        int n = w.length+1;
        int[][] dp = new int[n][k+1];

        for(int i=0; i < n; i++) {
            for(int j=0; j<=k; j++) {
                if(i == 0) {
                    dp[i][j] = 0;
                } else {
                    if(w[i-1] > j) {
                        dp[i][j] = dp[i-1][j];
                    } else {
                        dp[i][j] = Math.max(dp[i-1][j], v[i-1] + dp[i-1][j-w[i-1]]);
                    }
                }
            }
        }
        backtraceDP(dp, w, taken); // Stores path in taken
        return dp[n-1][k]; // Returns optimal value
    }
    

**It is possible to space-optimize knapsackDP by using 2 arrays instead of a matrix as follows:**

    private static int knapsackDP(int[] v, int[] w, int k, int[] taken) {
        int[] row0 = new int[k+1];
        int[] row1 = new int[k+1];
        
        for(int i=0; i<=w.length; i++) {
            for(int j=0; j<=k; j++) {
                if(i==0) {
                    row1[j] = 0;
                } else {
                    if(w[i-1] <= j) {
                        row1[j] = Math.max(row0[j], v[i-1]+ row0[j-w[i-1]]);
                    } else {
                        row1[j] = row0[j];
                    }
                }
            }
            for(int j = 0; j< row0.length; j++)
                row0[j] = row1[j];
        }
        
        return row1[k]; // Returns optimal value
    }

However if we do this we will have at most 2 rows 

    row0 =      0       0       0       0       8       10      10      10      15      18      18      18
    row1 =      0       0       0       4       8       10      10      12      15      18      18      19

With only this much information I am not sure how to do the backtrace. Hence approach 2


# Approach 2: Branch and Bound using DFS


