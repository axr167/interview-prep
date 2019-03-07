
# Divide and Conquer

## Overview

Divide and conquer is basically this:
- Split the input 
- Compute the best result for the inputs that have been split
- The answer is either:
  - The better answer of the 2 parts
  - The answer created by joining the 2 parts in some way.

## Complexity

The complexity of divide and conquer algorithms can be figured out using the Master theorem.

![Master-Theorem](https://i.imgur.com/6KqqNhC.png)

### How this theorem works

We split the input into a set number of parts until the input can no longer be split. Then we perform certain operations on the split parts to get the result.

Let us consider a few examples.


**Find maximum element in array**

We split array into 2 parts then for both parts we get the max element and return overall max.

- T(n) = 2 * T(n/2) + n^0

Here 0 < log_2(2) hence T(n) = n^log_b(a) => n^log_2(2) => n

**Mergesort**

We split array into 2 parts then for both parts we merge, processing elements one after the other

- T(n) = 2 * T(n/2) + n^1

Here 1 = log_2(2) hence T(n) = n^d log(n) = nlog(n)

# Problems

### Get max element of an array

Logic: Split array by 2. Max element is max of left side or max of right side. Choose the greater one.

	private static int max(int[] a, int s, int e) {
		if(s==e)
			return a[s];
		else {
			int mid = (s+e)/2;
			int left = max(a,s,mid);
			int right = max(a,mid+1,e);
			return Math.max(left, right);
		}
	}

### MergeSort

    TO-DO

### Best time to buy/sell stocks

Logic: max profit can be either in the left subarray or in the right subarray or it can be made by combining both subarrays.

Assume that f(left) gets max profit at left subarray and f(right) gets max profit from right subarray. Net max profit = either max(left, right) or it can be found by combining both. If we combine both, profit is right.max_element - left.min_element.

    private int find(int[] a, int s, int e) {
        if(s==e)
            return 0;
        else {
            // find best profit from left and right
            int mid = (s+e)/2;
            int left = find(a, s, mid);
            int right = find(a, mid+1, e);
            int local_max = Math.max(left, right);
            
            // merge by combining max_right and min_left
            int leftmin = a[s];
            int rightmax = a[mid+1];
            for(int i = s+1; i<=mid; i++)
                if(a[i] < leftmin)
                    leftmin = a[i];
            for(int i = mid+2; i<=e; i++)
                if(a[i] > rightmax)
                    rightmax = a[i];
            return Math.max(local_max, rightmax-leftmin);
        }    
    }


### Eugene's question (see youtube vid)

	int find(int[] a, int start, int end) {
		if(s == end) {
			return start+a[start];
		} else {
			int mid = (start+end)/2;
			int left = find(a, start, mid);
			int right = find(a, mid+1, end);
			if(left < mid+1)
				return left;
			else {
				return Math.max(left, right);
			}
		}
	}

### Largest area under histogram

    private int getArea(int[] a, int s, int e) {
        if(s==e)
            return a[s];
		
		// DIVIDE
        int mid = (s+e)/2;
        int left = getArea(a,s,mid);
        int right = getArea(a, mid+1, e);
        int lr_area = Math.max(left, right);
        
		// CONQUER
        int i = mid;
        int j = mid+1;
        int min = a[i];
        int area = 0;
        while(i>=s && j<=e) {
            min = Math.min(min, Math.min(a[i], a[j]));
            int localArea = min * (j-i+1);
            if(localArea > area)
                area = localArea;
            
            if(i<=s)
                j++;
            else if(j>=e)
                i--;
            else {
                if(a[j+1] > a[i-1])
                    j++;
                else
                    i--;
            }
        }
        return Math.max(area, lr_area);
    }
