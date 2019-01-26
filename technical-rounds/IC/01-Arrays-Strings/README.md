
## Arrays

Organizes data sequentially in memory. Fiexed size and has fast lookups and appends O(1) but expensive inserts and deletes O(n). They are building blocks for lots of other data structures.

Array slicing involves taking a subset from an array and allocating it to a new array. It is O(n) time and space and can be done as follows: int[] copy = Arrays.copyOfRange(array, 0, array.length);

An **in-place** algorithm is one that operates directly on the input and changes it instead of creating/returning a new object. These are destructive as original input is destroyed. Out of place algirithms are safer as it preserves the input however they consume extra space.

Dynamic arrays are variable size arrays and like its static counterparts it has O(1) lookups and appends but O(n) inserts and deletes. They are also cache friendly. However they have slow worst case appends.

## Problems

### 1. Merge intervals 

(see leetcode)

### 2. Reverse characters in array in place

trivial

### 3. Reverse words in place

    private static void reverse(char[] a, int l, int r) {
        while(l<r) {
            char temp = a[l];
            a[l] = a[r];
            a[r] = temp;
            l++;
            r--;
        }
    }
    private static void reverseWords(char[] a) {
        reverse(a, 0, a.length-1);
        int s = 0;
        for(int i=0; i<a.length; i++) {
            if(a[i] == ' ') {
                reverse(a, s, i-1);
                s = i+1;
            }
        }
        reverse(a, s, a.length-1);
    }
    
    public static void main(String []args){
        char[] message = { 'c', 'a', 'k', 'e', ' ',
                   'p', 'o', 'u', 'n', 'd', ' ',
                   's', 't', 'e', 'a', 'l' };
        reverseWords(message);
        System.out.println(message);
        
    }

### Merge 2 sorted arrays:

trivial - do merge from mergesort

**Merge k sorted arrays:**
- Method 1: If we use k pointers and select the min of the k pointers same as mergesort, we have a complexity of N * K
- Method 2: To make it better we need a better function to find min. So use a 'K' sized heap. Complexity now becomes N * log(k)




  
