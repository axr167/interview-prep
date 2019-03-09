

## Partitioning Problems

In these problems we try to partition an array into different segments. The technique used here is the 2 pointer technique (slow/fast method) where it terminates when fast pointer reaches n. Example problems are given below.

### Move odd numbers to end of list

- Here we must partition array into 2 groups even and odd. 
- Slow pointer always points to last element of even partition. 
- So when fast pointer encounters an even number, it must be placed immediately after the even partition to maintain partitioning. In this case, we increment slow pointer, swap values of slow and fast pointers and continue.

	private void partition(int[] a) {
		int i = -1;
		int j = 0;
		while(j<a.length) {
			if(a[j]%2==1)
				j++;
			else {
				i++;
				int temp = a[i];
				a[i] = a[j];
				a[j] = temp;
				j++;
			}
		}
	}
