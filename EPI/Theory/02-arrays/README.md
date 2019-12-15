
# Array based problems

## Partitioning problems

In these types of problems, we need to partition arrays into 2 (or 3) types where each type has a similar set of elements. The idea here is to use both ends of the array. So we have 3 pointers:

- A left pointer: Everything behind this pointer in the left direction is of type 1.
  - At the start its value = 0 because we have not processed any elements yet hence there is nothing behind this pointer.
- A right pointer: Everything behind this pointer in the right direction is of type 2.
  - At the start its value = a.length -1 because we have not processed any elements yet hence there is nothing behind this pointer.
- The current pointer: This is the pointer that iterates over the array sequentially. It starts at 0 - the first array element.

### The logic:

If we see element of type 1, swap current element with element that left pointer points to then increment left pointer and current

If we see element of type 2, swap current element with element that right pointer points to then decrement right pointer. Leave current as is. We do not increment current here because current has not seen or examined the swapped element yet.

Repeat while current <= right pointer.

**Let us consider dividing an array into 2 parts such that all odd numbers are on the left and all even numbers are to the right.** 

The code to do this is shown below.

    public void solve() {
        int[] a = new int[]{1,2,3,4,5,6,7};

        int l = 0; int r = a.length-1; // left and right pointers
        int c = 0; // current pointer

        while(c <= r) {
            if(a[c] %2 == 1) {
                swap(a, l, c);
                l++;
                c++;
            } else {
                swap(a, r, c);
                r--;
            }
        }
        System.out.println(Arrays.toString(a));
    }

The exact same logic can be used to divide the elements into 3 parts as well (type 1, unclassified, type 2). This is called the dutch national flag problem. Consider the problem below:

**Sort an array of 0s 1s and 2s in O(n) time**

The code to do this is shown below:

    public void solve() {
        int[] a = new int[]{1,1,0,2,0,1,2,2,0,1,1,2};

        int l = 0; int r = a.length-1; // left and right pointers
        int c = 0; // current pointer

        while(c <= r) {
            if(a[c] == 0) {
                swap(a, l, c);
                l++;
                c++;
            } else if (a[c] == 1) {
                c++;
            }  else if (a[c] == 2) {
                swap(a, r, c);
                r--;
            }
        }
        System.out.println(Arrays.toString(a));
    }

For array problems it is incredibly easy to make off by one errors. For example the above code would not work if we did while(c < r).
