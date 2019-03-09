

## Partition Problems

In these problems we try to partition an array into different segments. The technique used here is the 2 pointer technique (slow/fast method) where it terminates when fast pointer reaches n. 

The slow pointer points to the last index of a partition and the fast pointer iterates through the list. If it encounters something that should belong to the slow pointer, we increment first partition put new element at end of partition and continue.

Example problems are given below.

### Move odd numbers to end of list

- Here we must partition array into 2 groups: even and odd. 
- Slow pointer always points to last element of even partition (-1 at start). 
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

### Remove duplicates from sorted list

- Here we must partition array into 2 groups: Without duplicates and other (containing duplicates).
- Slow pointer points to last emelent of without duplicate partition (0 at start because first element will always be unique)


        private int partition(int[] a) {
            int i = 0;
            int j = 1;
            while(j<a.length) {
                if(a[j] == a[i])
                    j++;
                else {
                    i++;
                    int temp = a[i];
                    a[i] = a[j];
                    a[j] = temp;
                    j++;
                }
            }
            return i+1;
        }

### Remove duplicates from sorted array such that duplicates appeared at most twice

- Partition array into 2 groups: Part where duplicates appear at most twice and other.
- Slow pointer points to last element of partition 1. (Starts at element 1 because first 2 elements will always be valid)

        private int partition(int[] a) {
            int i = 1;
            int j = 2;
            while(j<a.length) {
                if(a[j] == a[i] && a[i] == a[i-1])
                    j++;
                else {
                    i++;
                    int temp = a[i];
                    a[i] = a[j];
                    a[j] = temp;
                    j++;
                }
            }
            return i+1;
        }

### Partition array according to pivot (quicksort)

- Partition array into 2 groups: Part with elements less than pivot and part with elements greater than pivot.
- Slow pointer points to last element of partition 1.

        private static void partition(int[] a, int pivot) {
            int i = -1;
            int j = 0;
            while(j<a.length) {
                if(a[j] > pivot)
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

### Sort array containing values 0,1,2 - Partition array into 3 parts
