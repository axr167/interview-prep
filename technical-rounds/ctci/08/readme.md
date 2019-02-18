
### 1. Triple Step: You can jump 1 step, 2 steps or 3 steps at a time. Count no. of ways to run up the steps:

    // f(n) = 1,2,4 //if(n==1,2,3)
    // f(n) = f(n-1)+f(n-2)+f(n-3) //otherwise
    
    private int count(int n) {
      int c1 = 1;
      int c2 = 2;
      int c3 = 4;
      int c;
      for(int i=4; i<n; i++) {
        c = c1+c2+c3;
        int temp = c3;
        c3 = c;
        int temp2 = c2;
        c2 = temp;
        c1 = temp2;
      }
      return c;
    }
    
### 2. Robot in a grid: Min path sum (leetcode)

### 3. Magic index

Q1. Magic index is when a[i] = i in an array. Find it if a has all distinct values and is sorted.

    private static int get(int[] a) {
      int lo = 0;
      int hi = a.length-1;
      while(lo<=hi) {
        int mid = (lo+hi)/2;
        if(a[mid] == mid)
          return mid;
        else if(a[mid]<mid)
          lo = mid+1;
        else
          hi = mid-1;
      }
      return -1;
    }

Q2. What if there are elements are not distinct?
