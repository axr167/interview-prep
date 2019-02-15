
# Binary Search

## The code:

The code for binary search is below

**Iterative:**

    private int f(int[] a, int t) {
          int lo = 0;
          int hi = a.length-1;
          while(lo <= hi) {
              int mid = (lo+hi)/2;
              if(a[mid] == t)
                  return mid;
              if(a[mid] > t)
                  hi = mid-1;
              else
                  lo = mid+1;
          }  
          return -1;
      }

**Recursive:**

    int f(int[]a, int lo, int hi, int t) {
        if(lo<=hi) {
            int mid = (lo+hi)/2;
            if(a[mid] == t)
                return mid;
            else if (a[mid] > t)
                return f(a, lo, mid-1, t);
            else
                return f(a, mid+1, hi, t);
        } else
            return -1;
    }


**Important: Consider the edge cases**

In divide and conquer algorithms it is important to check edge cases and to understand when the algorithm terminates. Let us also look at quick/mergesort algorithms.

Mergesort is:

    if (l < r) { 
        int m = (l+r)/2; 
        mergesort(arr, l, m); 
        mergesort(arr , m+1, r); 
        merge(arr, l, m, r); 
    }

Quicksort is:

    if (low < high) { 
        int p = partition(arr, low, high); 
        sort(arr, low, p-1); 
        sort(arr, p+1, high); 
    }

**The comparison:**

Binary Search:

- We do binary search if lo<=hi because it is possible that only 1 element is left and it might be answer
- We do mid+1, mid-1 as lo/hi values because we have already checked mid hence it can never have the desired value

Mergesort:

- We do it if lo<hi because single value arrays are already sorted
- We do merge cases f(lo, mid) and f(mid+1,hi) because mid element must be included to sort

Quicksort

- We do it if lo<hi because single value arrays are already sorted
- We do merge cases f(lo, p-1) and f(p+1,hi) because p element sorted in place after calling partition.

### When to consider using binary search?

- When array is in sorted format.
- When solution is n^2 or more and sorting may result in better solution.

# Problems

## 1. Fixed rotation point

