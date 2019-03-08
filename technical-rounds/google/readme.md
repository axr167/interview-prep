
# LC hards

### Max Area Under Histogram


Explanation:

Divide the graph by 2. Max area is either max area on the left, Max area on the right or area formed by combining the 2.

    private int getArea(int[] a, int s, int e) {
        if(s==e)
            return a[s];
        int mid = (s+e)/2;
        int left = getArea(a,s,mid);
        int right = getArea(a, mid+1, e);
        int lr_area = Math.max(left, right);
        
        int i = mid;
        int j = mid+1;
        int min = a[i];
        int area = 0;
        while(i>=s && j<=e) {
            min = Math.min(min, Math.min(a[i], a[j]));
            int localArea = min * (j-i+1);
            if(localArea > area)
                area = localArea;
            
            if(i<=s)
                j++;
            else if(j>=e)
                i--;
            else {
                if(a[j+1] > a[i-1])
                    j++;
                else
                    i--;
            }
        }
        return Math.max(area, lr_area);
    }

## Trapping rainwater

Logic:

Find maximum bar max_bar. Then individually find water stored in left of maximum and right of maximum.

To find water on each side: 
- let us have a variable keeping track of maximum left bar and maximum right bar seen so far.
- Iterate over the graph from each end to max_bar
- If current bar is greater than max_l, make max_l the current bar
- if current bar is less than max_l, water can be filled on top of current bar (because it lies between max_l and max_bar)
    - water that can be added is max_l - curreent_height.
- Do the same for right.

        class Solution {
            public int trap(int[] height) {
                if(height.length <=2)
                    return 0;
                int max = Integer.MIN_VALUE;
                int max_index = -1;

                // Get max bar
                for(int i=0; i<height.length; i++) {
                    if(height[i] > max) {
                        max = height[i];
                        max_index = i;
                    }
                }

                int water = 0;

                // water at left of max
                int max_left = height[0];
                for(int i=0; i<max_index; i++) {
                    if(height[i] > max_left)
                        max_left = height[i];
                    else
                        water += max_left-height[i];
                }

                // water at right of max
                int max_right = height[height.length-1];
                for(int i=height.length-1; i>max_index; i--) {
                    if(height[i] > max_right)
                        max_right = height[i];
                    else
                        water+=max_right-height[i];
                }

                return water;

            }
        }
