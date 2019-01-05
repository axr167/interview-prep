

# 1. 0/1 Knapsack Problem

Brute force:

    public class Solution{

        private static int knapsack(int[] w, int[] v, int W, int V, int current) {
            if(current == w.length) {
                return V;
            }
            if(W >= w[current])
                return Math.max( knapsack(w,v,W,V,current+1), 
                            knapsack(w,v,W-w[current], V+v[current], current+1) );
            return knapsack(w,v,W,V,current+1);
        }

        public static void main(String []args){
            int[] w = {4,1,2,3,2,2};
            int[] v = {5,8,4,0,5,3};

            int W = 3;

            System.out.println(knapsack(w,v,W,0,0));

        }
    }

    - Moving variable current moves from 0 to w.length
    - There are 2 subproblems
        - knapsack(w,v,W,V,current+1)
        - knapsack(w,v,W-w[current], V+v[current], current+1)
    - Notice that subproblems overlap because
        - moving variable from subproblem 1 moves from 0 to w.length
        - moving variable from subproblem 2 moves from 0 to w.length
    - Hence we must use DP. Size of cache = 0 to w.length-1.
