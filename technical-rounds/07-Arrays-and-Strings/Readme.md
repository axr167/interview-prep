

## Table of Contents

1. [Partition Problems](#Partition-Problems)
2. [Converging Problems](#Converging-Problems)

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

## Getting subarray/substring that satisfies given condition

## Prefix sum

## Reversing the array/string (next permutation, rotation etc)

## Greedy and dp methods

## Printing gymnastics
