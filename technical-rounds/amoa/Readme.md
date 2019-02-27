
## Table of Contents

1. [Analog Problems](#Analog-Problems)
2. [Graphs](#Graphs)

## Analog Problems

### 2 Sum

**2-Sum with duplicates**

### k-closest points to origin

Logic: Create max heap for points. If heap size is less than K, add point to heap. Otherwise get point with longest distance from heap using heap.peek(). If current point has a lesser distance than max point, remove the max point from heap and put current point instead.

Complexity analysis:

Space: k because the heap size is never over k
Time: n * log(k). We process n elements and there are k elements in the heap.

    public int[][] kClosest(int[][] points, int K) {
        int[][] res = new int[K][2];
        
        Comparator<int[]> c = new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                if ( (a[0]*a[0] + a[1]*a[1]) > (b[0]*b[0] + b[1]*b[1]) )
                    return -1;
                else if ( (a[0]*a[0] + a[1]*a[1]) < (b[0]*b[0] + b[1]*b[1]) )
                    return 1;
                else
                    return 0;
            }
        };
        // Max heap
        PriorityQueue<int[]> pq = new PriorityQueue<>(c);
        for(int[] p: points) {
            if(pq.size() < K)
                pq.add(p);
            else {
                int[] b = pq.peek();
                if( (p[0]*p[0] + p[1]*p[1]) < (b[0]*b[0] + b[1]*b[1]) ) {
                    pq.remove();
                    pq.add(p);
                }
            }
        }
        int i = 0;
        while(!pq.isEmpty()) {
            res[i] = pq.remove();
            i++;
        }
        return res;
    }

## Graphs
