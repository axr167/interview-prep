

# 1. 0/1 Knapsack Problem

Brute force:

    private static int knapsack(int[] w, int[] v, int W, int V, int current) {
        if(current == w.length) {
            return V;
        }
        if(W >= w[current])
            return Math.max( knapsack(w,v,W,V,current+1), 
                        knapsack(w,v,W-w[current], V+v[current], current+1) );
        return knapsack(w,v,W,V,current+1);
    }
