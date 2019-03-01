
## Problems

### 1. Absolute value sort

Given an array of integers arr, write a function absSort(arr), that sorts the array according to the absolute values of the numbers in arr. If two numbers have the same absolute value, sort them according to sign, where the negative numbers come before the positive numbers.

	  static int partition(int[] a, int l, int r) {
	    int p = r;
	    int i = l-1;
	    int j = l;
	    while(j<r) {
	      if(Math.abs(a[j]) >= Math.abs(a[p])) {
		j++;
	      } else {
		i++;
		int temp = a[i];
		a[i] = a[j];
		a[j] = temp;
		j++;
	      }
	    }
	    i++;
	    int temp = a[i];
	    a[i] = a[p];
	    a[p] = temp;
	    return i;
	  }

	  static void quicksort(int[] a, int s, int e) {
	    if(s < e){
	    	int p = partition(a, s, e);
	      	quicksort(a,s,p-1);
	      	quicksort(a,p+1,e);
	    }
	  }

	static int[] absSort(int[] arr) {
		quicksort(arr, 0, arr.length-1);
	    	int j = 0; int i = 1;
	    	while(i<arr.length) {
	      		if(Math.abs(arr[j]) == Math.abs(arr[i]) && arr[j] > arr[i]) {
				int temp = arr[i];
				arr[i] = arr[j];
				arr[j] = temp;
	      	}
	      	i++;
	      	j++;
	    }
	    return arr;
	}
