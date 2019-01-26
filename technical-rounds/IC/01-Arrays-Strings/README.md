
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

### Single Riffle Shuffle

- Top of first deck must always have top of either left half or top of right half
- Each iteration remove top of deck and top of either left/right half (by incrementing pointers)
- If neither matches then return false
- If all 3 pointers = array.length return true.

**Bonus**

1. This assumes shuffledDeck contains all 52 cards. What if we can't trust this (e.g. some cards are being secretly removed by the shuffle)?

2. This assumes each number in shuffledDeck is unique. How can we adapt this to rifling arrays of random integers with potential repeats?

3. Our solution returns true if you just cut the deck—take one half and put it on top of the other. While that technically meets the definition of a riffle, what if you wanted to ensure that some mixing of the two halves occurred?

4. Our solution iterates through the decks from front to back. Would our algorithm work if we iterated from the back towards the front? Which approach is cleaner?
  
