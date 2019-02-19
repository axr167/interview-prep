
## Types of problems encountered:

**Slow pointer/Fast pointer approach**
- Here initialize slow and fast pointers
- Ends when either slow = fast (loop in list etc) or when fast = end (remove duplicates)
- i can move slower or as fast as j

**Start/End approach**

- Initialize p1 to start of array
- Initialize p2 to end of the array
- Either increment p1 or decrement p2 to maximize/minimize something and keep doing so until p1>=p2
- Examples include trapping rainwater and 2/3/4-sum.

**Slow/Fast pointer with memory**
- This is used in problems where you must find a subarray/substring/any continuous block such that the block satisfies certain conditions.
- Have slow/fast pointer and a hashmap/hashset as supporting cache
- Increment fast pointer adding element to cache until the condition is violated.
- When condition is violated, increment slow pointer until condition is satisfied again removing things from cache as i is incremented
- Do until fast pointer reaches end.

### 1. Put array duplicates at end of sorted array

Slow pointer/fast pointer approach.

If i == j then increment j.
If i is not equal to j then increment i and replace i,j values

    public int removeDuplicates(int[] a) {
        if(a.length == 0)
            return 0;
        int i = 0; int j = 1;
        while(j!=a.length) {
            if(a[i] == a[j])
                j++;
            else {
                i++;
                a[i] = a[j];
                j++;
            }
        }
        return i+1;
    }

### 2. Container with most water

Start/end approach. Have 2 pointers at each end and check height of both pointers. If h(p1) > h(p2) decrement p2 otherwise increment p1.

    public int maxArea(int[] a) {
        int p1 = 0;
        int p2 = a.length-1;
        int max_vol = 0;
        while(p1 < p2) {
            int vol = Math.min(a[p1],a[p2])*(p2-p1);
            if(vol > max_vol)
                max_vol = vol;
            if(a[p1] > a[p2])
                p2--;
            else
                p1++;
        }
        return max_vol;
    }
