
### 1. Sorted merge

//mergesort

## 2. Group Anagrams

// See leetcode

### 3. Search in rotated array

	private static int pivot(int[] a) {
		int lo = 0;
		int hi = a.length-1;
		if(a[lo] < a[hi])
			return lo;
		while(lo <= hi) {
			int mid = (lo+hi)/2;
			if(a[mid+1] < a[mid])
				return mid+1;
			if(a[mid] < a[lo])
				hi = mid-1;
			else
				lo = mid+1;
		}
		return -1;
	}
  
**Follow up: What if there are duplicates?**

In this case we have to search both sides

