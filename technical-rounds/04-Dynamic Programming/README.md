

# 1. 0/1 Knapsack Problem


Main function:

    public static void main(String []args){
        int[] w = {4,1,2,3,2,2};
        int[] v = {5,8,4,0,5,3};

        int W = 3;

        System.out.println(knapsack(w,v,W,0,0));

    }

Brute force:


        /* BRUTE FORCE */
        private static int knapsack(int[] weight, int[] value, int totalWeight, int currentItem) {
            // BASE CASE
            if(currentItem == weight.length)
                return 0;
            // RECURSIVE STEP
            else {
                if(totalWeight >= weight[currentItem])
                    return Math.max( knapsack (weight, value, totalWeight, currentItem+1), 
                             value[currentItem] + knapsack (weight, value, totalWeight - weight[currentItem], currentItem+1) );
                else
                    return knapsack (weight, value, totalWeight, currentItem+1);
            }
        }

Notes:

    - Moving variables that determine output V are as follows:
        - current: moves from 0 to w.length
        - W: moves from W to 0
    - There are 2 subproblems
        - knapsack(w,v,W,V,current+1)
        - knapsack(w,v,W-w[current], V+v[current], current+1)
    - Notice that subproblems overlap
    - Hence we must use DP. Size of cache = 0 to w.length-1.
    

Hence the code is:

    private static int mem_knap(int[] weight, int[] value, int totalWeight, int currentItem, int[][] dp) {
        // BASE CASE
        if(currentItem == weight.length){
            return 0;
        }
        // RECURSIVE STEP
        else {
            // If cache has answer return it else recurse and store answer in cache
            if (dp[currentItem][totalWeight] != -1)
                return dp[currentItem][totalWeight];
            else{
                if(totalWeight >= weight[currentItem])
                    dp[currentItem][totalWeight] =  Math.max( mem_knap (weight, value, totalWeight, currentItem+1, dp), 
                            value[currentItem]+ mem_knap(weight, value, totalWeight-weight[currentItem], currentItem+1,dp));
                else
                    dp[currentItem][totalWeight] =  mem_knap (weight, value, totalWeight, currentItem+1, dp);
            }

            return dp[currentItem][totalWeight];
    }
  
This can be converted into bottom up as follows:

    //Placeholder for code
