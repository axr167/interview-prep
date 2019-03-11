

## Table of Contents

1. [Partition Problems](#Partition-Problems)
2. [Converging Problems](#Converging-Problems)
3. [Conditional Subarray Problems](#Conditional-Subarray-Problems)

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

### Sort array containing values 0,1,2 (Dutch national flag problem)

- Partition array into 3 groups: 0, 1, 2.
- We have 3 pointers: Pointer at end of partition 1 (i), iterating pointer for mid partition (j), pointer at end of last partition (j)

        private void partition(int[] a) {
            int i = -1;
            int j = 0;
            int k = a.length-1;

            while(j<=k) {
                if(a[j] == 1)
                    j++;
                else if(a[j] == 0) {
                    i++;
                    int temp = a[i];
                    a[i] = a[j];
                    a[j] = temp;
                    j++;
                } else if(a[j] == 2) {
                    int temp = a[j];
                    a[j] = a[k];
                    a[k] = temp;
                    k--;
                }
            }
        }

## Converging Problems

A type of 2 pointer problem where we have a left pointer and a right pointer. We then either start from the ends and converge to the middle or we start at the middle and go towards the end. It is useful for things like finding areas etc.

Example problems are below:

### Container with most water

Logic: Start at both ends. If a[left] < a[right] increment left else decrement right

    public int maxArea(int[] a) {
        int i = 0;
        int j = a.length-1;
        int area = 0;
        while(i<j) {
            area = Math.max(area, Math.min(a[i], a[j])*(j-i));
            if(a[i] < a[j])
                i++;
            else
                j--;
        }
        return area;
    }

### Trapping rainwater

Logic: Find max height - let this act as the middle element. Converge towards the middle calculating water on each step. The water is left/right height-current height if current height is less than left/right height.

    public int trap(int[] a) {
        
        if(a.length <=2) return 0;
        
        int water = 0;
        int max = a[0];
        int mid = 0;
        
        // get mid
        for(int i=0; i<a.length; i++) {
            if(a[i] > max) {
                max = a[i]; mid = i;
            }
        }
        
        int left = 0;
        int i = 0;
        while(i<mid) {
            if(a[i] > a[left])
                a[left] = a[i];
            else
                water += (a[left] - a[i]);
            i++;
        }
        
        int right = a.length-1;
        int j = a.length-1;
        while(j>mid) {
            if(a[j] > a[right])
                a[right] = a[j];
            else
                water += (a[right] - a[j]);
            j--;
        }
        return water;
    }

### Area under histogram (divide and conquer method)

Logic: Find max left, max right areas. Then find areas formed by combining the two. To do this, start at the mid and expand to ends.

    private int getArea(int[] a, int s, int e) {
        if(s==e)
            return a[s];
        
        int mid = (s+e)/2;
        int left = getArea(a,s,mid);
        int right = getArea(a,mid+1, e);
        
        int i = mid; 
        int j = mid+1; 
        int min = Math.min(a[i], a[j]);
        int area = 0;
        
        while(i>=s && j<=e) {
            min = Math.min(min, Math.min(a[i], a[j]));
            int current_area = min*(j-i+1);
            area = Math.max(area, current_area);
            if(i <= s) 
                j++;
            else if(j >= e) 
                i--;
            else if(a[i-1] > a[j+1]) 
                i--;
            else 
                j++;
        }
        return Math.max(area, Math.max(left, right));
    }

## Conditional Subarray Problems

These are generally 2 pointer problems that use a map/set. The idea is that the fast pointer iterates through the array storing values into the map/set until the condition is violated. Result is stored and then the slow pointer iterates through the array until the condition is satisfied again.

### Length of longest substring without repeating characters

- i points to first character of a valid substring, j points to character after last character of a valid substring.
- if the set does not contain j, then record the result and increment i until the condition becomes valid again 
- Condition is: Character at j should be removed from set

        public int lengthOfLongestSubstring(String s) {
            if(s.length()<=1) return s.length();

            Set<Character> set = new HashSet<>();
            set.add(s.charAt(0));
            int res = 1;
            int i=0; int j=1;

            while(j<s.length()) {
                if(!set.contains(s.charAt(j))) {
                    set.add(s.charAt(j));
                    j++;
                } else {
                    res = Math.max(res, set.size());
                    while(set.contains(s.charAt(j))) {
                        set.remove(s.charAt(i));
                        i++;
                    } 
                }
            }
            return Math.max(res, set.size());
        }

### Smallest subarray containing a single duplicate

- Keep iterating until j reaches a duplicate. Then iterate i until i reaches the first duplicate. Record results and continue.
- Do until j reaches string.length

        private int smallest(int[] a) {
            Set<Integer> set = new HashSet<>();
            int cost = a.length;
            int i =0; int j = 1;
            set.add(a[0]);
            while(j<a.length) {
                if(!set.contains(a[j])) {
                    set.add(a[j]);
                    j++;
                } else {
                    while(set.contains(a[j])) {
                        if(a[i] == a[j]) {
                            cost = Math.min(cost, (j-i+1));
                        }
                        set.remove(a[i]);
                        i++;
                    }
                }
            }
            return cost;
        }

### Longest substring with at most k distinct characters

- Use hashmap to record character count. If character encountered before, increment j and increment character count in map.
- If map size = k, then record longest. Reduce char count at i and increment i. 
- If char count for any char is less than k, then remove it for map. Do this until map size is less than k
- At the end compare current with the substring and return the max of the 2.

        public int lengthOfLongestSubstringKDistinct(String s, int k) {
            if(s.length() ==0 || k==0)
                return 0;
            int i=0; int j = 1;
            int longest = 0;
            Map<Character, Integer> map = new HashMap<>();
            map.put(s.charAt(i), 1);

            while(j<s.length()) {
                if(map.containsKey(s.charAt(j))) {
                    map.put(s.charAt(j), map.get(s.charAt(j))+1);
                    j++;
                } else {
                    if(map.size() == k) {
                        longest = Math.max(longest, (j-i));
                        while(map.size() == k) {
                            map.put(s.charAt(i), map.get(s.charAt(i))-1);
                            if(map.get(s.charAt(i)) == 0)
                                map.remove(s.charAt(i));
                            i++;
                        }
                    } else {
                        map.put(s.charAt(j), 1);
                        j++;
                    }
                }
            }
            return Math.max(longest, j-i);
        }


## Prefix sum

## Reversing the array/string (next permutation, rotation etc)

## Greedy and dp methods

## Printing gymnastics
