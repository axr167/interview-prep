
# LC hards for google

### Max Area Under Histogram

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

