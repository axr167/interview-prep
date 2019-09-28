
## Problem: Solve Knapsack for a bunch of datasets

Need to return the following:

    Optimal_value     Are_we_sure_its_optimal

    0_1_Array_for_picked_items

Example

    19  1
    0   0   1   1
    
- This means 19 is the optimal value, 1 means that we are 100% sure 19 is optimal and not just a greedy approximation
- The bottom array means items 0,1 were not picked and items 2,3 were picked to get optimal value


## Approach 1: Dynamic Programming (not optimized for space)

**The recurrence is:**

    Let n be the nth item 
    let c be the remaining capacity in the knapsack

    f(n, c) = 0             // if n=0
    f(n, c) = f(n-1, c)     // if weight[n] > c
    f(n, c) = max(f(n-1, c), value[n] + f(n-1, c-weight[n]))  // if weight[n] <= c

**Logic to get the items is (for ks_4_0)**

1. The matrix dp[][] is as follows:

      i\j   |    0       1       2       3       4       5       6       7       8       9       10      11    
    \----------------------------------------------------------------------------------------------------------
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

2. Row 4 (index 3) in dp[][] corresponds to item 3 so I compare the dp[3][9] (bottom right corner) with dp[2][9]. Since both values are the same I know **item 3 was not chosen**. I go to dp[2][9].
3. I compare dp[2][9] to dp[1][9]. Since the values are different, I know **item 2 was chosen**. I go to dp[1][9 - weight of item 2] => dp[1][4].
4. I compare dp[1][4] with dp[0][4]. The values are different so I know **item 1 was chosen**. I go to dp[0][4 - weight of item 1] => dp[0][0].
5. dp[0][0] is the terminal state so I return.


DP solution with time and space O(n^2):


