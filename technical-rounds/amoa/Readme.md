
## Table of Contents

1. [Analog Problems](#Analog-Problems)
2. [Graphs](#Graphs)
3. [DP](#DP)
4. [Design](#Design)
5. [2Pointer](#2Pointer)
6. [Recursion](#Recursion)
7. [Tree](#Tree)

## Analog Problems

### 1. 2 Sum

### 2. k-closest points to origin

Logic: Create max heap for points. If heap size is less than K, add point to heap. Otherwise get point with longest distance from heap using heap.peek(). If current point has a lesser distance than max point, remove the max point from heap and put current point instead.

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

### 3. Most common words in paragraph

Logic: Preprocess string by converting to lowercase, replacing punctuation with whitespace and splitting by \\s+

Time: O(n) as we go through each word
Space: O(n) to store in map

    class Solution {
        public String mostCommonWord(String p, String[] banned) {
            Set<String> set = new HashSet<>();
            for(String s: banned)
                set.add(s);
            p = p.toLowerCase().replaceAll("[!?',;.]"," ");
            String[] arr = p.split("\\s+");
            Map<String, Integer> map = new HashMap<>();
            for(String s: arr) {
                if(!set.contains(s)) {
                    if(!map.containsKey(s))
                        map.put(s,1);
                    else
                        map.put(s,map.get(s)+1);
                }
            }
            int max = 0;
            String res = "";
            for(String s: map.keySet()) {
                if(map.get(s) > max) {
                    max = map.get(s);
                    res = s;
                }
            }
            return res;
        }
    }

### 4. Merge 2 sorted linked lists without extra space

Logic: Set start pointers to each list and head pointer and have a start pointer. Set head pointer to smaller of l1 and l2, incrementing smaller node to node.next. Set current = head and in a while loop assign current = smaller of l1,l2.

Time: O(n)
Space: O(1)

    class Solution {
        public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
            if(l1 == null && l2 == null)
                return null;
            else if(l1==null)
                return l2;
            else if(l2 == null)
                return l1;
            ListNode s;
            if(l1.val < l2.val) {
                s = l1; l1 = l1.next;
            } else {
                s=l2; l2=l2.next;
            }
            ListNode head = s;

            while(l1!=null || l2!= null) {
                if(l1!=null && l2!= null) {
                    if(l1.val <l2.val) {
                        s.next = l1;
                        s = s.next;;
                        l1 = l1.next;
                    } else {
                        s.next = l2;
                        s = s.next;
                        l2 = l2.next;
                    }
                } else if(l1 == null) {
                    s.next = l2;
                    break;
                } else {
                    s.next = l1;
                    break;
                }
            }
            return head;
        }
    }

### 5. Copy list with random pointer

Logic: Map each node to a new node. Then for each new node created, link based on values stored in map
Time: O(n)
Space: O(n)

        class Solution {
            public Node copyRandomList(Node head) {
                Map<Node, Node> map = new HashMap<>();
                Node current = head;
                while(current!=null) {
                    map.put(current, new Node());
                    current = current.next;
                }
                for(Node n: map.keySet()) {
                    Node x = map.get(n);
                    x.val = n.val;
                    x.next = map.get(n.next);
                    x.random = map.get(n.random);
                }
                return map.get(head);
            }
        }

### 6. Largest area under histogram (Stack based)

### 7. Next permutation


## Graphs

### 1. Number of Islands

Logic: Iterate over matrix. When we find a 1 which is not visited, do bfs

Time: O(m * n). Both Bfs loop take m * n time.
Space: m * n space taken by BFS

    class Solution {

        private boolean isValid(char[][] a, int i, int j) {
            if(i<0||j<0||i>=a.length||j>=a[0].length)
                return false;
            return true;
        }

        private void bfs(int i, int j, char[][] a, Set<Point> set) {
            Queue<Point> q = new LinkedList<>();
            Point p = new Point(i,j);
            set.add(p);
            q.add(p);
            while(!q.isEmpty()) {
                p = q.remove();
                int x = p.i;
                int y = p.j;

                if(isValid(a,x+1, y) && a[x+1][y] == '1' && !set.contains(new Point(x+1, y))) {
                    Point n = new Point(x+1, y);
                    set.add(n);
                    q.add(n);
                }

                if(isValid(a,x-1, y) && a[x-1][y] == '1' && !set.contains(new Point(x-1, y))) {
                    Point n = new Point(x-1, y);
                    set.add(n);
                    q.add(n);
                }

                if(isValid(a, x, y+1) && a[x][y+1] == '1' && !set.contains(new Point(x, y+1))) {
                    Point n = new Point(x, y+1);
                    set.add(n);
                    q.add(n);
                }

                if(isValid(a, x, y-1) && a[x][y-1] == '1' && !set.contains(new Point(x, y-1))) {
                    Point n = new Point(x, y-1);
                    set.add(n);
                    q.add(n);
                }
            }
        }

        public int numIslands(char[][] a) {
            int counter = 0;
            Set<Point> set = new HashSet<>();
            for(int i=0; i<a.length; i++) {
                for(int j=0; j<a[0].length; j++) {
                    if(a[i][j] == '1' && !set.contains(new Point(i,j))) {
                        counter++;
                        bfs(i,j,a,set);
                    }
                }
            }
            return counter;
        }

        class Point{
            int i; int j;
            public Point(int i, int j) {
                this.i=i; this.j=j;
            }
            @Override
            public boolean equals(Object o) {
                if(this == o) return true;
                if(!(o instanceof Point)) return false;
                Point p = (Point) o;
                return i==p.i && j==p.j;
            }
            @Override
            public int hashCode(){
                final int prime = 31;
                int res = 1;
                res = prime*res+i;
                res = prime*res+j;
                return res;
            }
        }
    }
    
## DP

### 1. Longest Palindromic Substring

Logic: if s is a palindrome return s. Otherwise the longest palindrome is either in substring created by removing 1 character from front or from substring created by removing 1 char from back.

Time: O(n^3). O(n^2) because 2 for loops and another O(n) to run the isP() function
Space: O(n)

        class Solution {
            /*
                f(i,j) = s // if(isP(s))
                f(i,j) = max(f(i+1,j), f(i,j-1)) //otherwise
            */
            public String longestPalindrome(String s) {
                if(s.length() < 2)
                    return s;
                String[] row1 = new String[s.length()];
                String[] row2 = new String[s.length()];

                for(int i=s.length()-1; i>=0; i--) {
                    for(int j=i; j<s.length(); j++) {
                        if(isP(s.substring(i,j+1)))
                            row1[j] = s.substring(i,j+1);
                        else {
                            if(row2[j].length() > row1[j-1].length())
                                row1[j] = row2[j];
                            else
                                row1[j] = row1[j-1];
                        }
                    }
                    for(int j=0; j<row1.length; j++)
                        row2[j] = row1[j];
                }
                return row1[s.length()-1];
            }

            private boolean isP(String s) {
                int i = 0;
                int j = s.length()-1;
                while(i < j) {
                    if(s.charAt(i) != s.charAt(j))
                        return false;
                    i++;
                    j--;
                }
                return true;
            }
        }

## Design

### 1. LRU Cache

Logic: Maintain Map<Key, Node> for constant time get operations and Double linked list for constant time modify operations. If list size is more than capacity, remove last node. Each time you add or get, add node to start of list.

Time: O(1) for all operations
Space: O(n) for map and list

        class LRUCache {
            class Node{
                int val;
                int key;
                Node next;
                Node prev;
                public Node() {
                    this.next = null; this.prev = null;
                }
                public Node(int key, int val) {
                    this();
                    this.val = val; this.key = key;
                }
            }
            Node head;
            Node tail;
            int capacity;
            int size;
            Map<Integer, Node> map;
            public LRUCache(int capacity) {
                this.head = new Node();
                this.tail = new Node();
                head.next = tail;
                tail.prev = head;
                this.capacity = capacity;
                this.size = 0;
                map = new HashMap<Integer, Node>();
            }
            public int get(int key) {
                if(!map.containsKey(key))
                    return -1;
                Node current = map.get(key);
                Node prev = current.prev;
                Node next = current.next;
                prev.next = next;
                next.prev = prev;

                Node h_next = head.next;
                head.next = current;
                current.prev = head;
                current.next = h_next;
                h_next.prev = current;

                return current.val;

            }
            public void put(int key, int value) {
                Node c = new Node(key, value);
                Node next = head.next;
                head.next = c;
                c.prev = head;
                c.next = next;
                next.prev = c;
                map.put(key, c);
                size++;
                if(size > capacity) {
                    Node del = tail.prev;
                    int del_key = del.key;
                    map.remove(del_key);
                    Node prev = del.prev;
                    del.prev = null; del.next = null;
                    tail.prev = prev;
                    prev.next = tail;
                    size--;
                }
            }
        }

### 2. Find Median from Data Stream

Logic: Median can be found by sorting the list then if it is odd, pick middle element and if it is even pick largest element of 1st half of list and smallest element of 2nd half of the list. Maintain 2 heaps - max heap for first half, min heap for second half.
If first element, put it in max heap. If element is larger than max heap's top, add to min heap. If one heap is larger that the other by more than 1 element pop the larger heap into the smaller heap.

Time: log(n) to add, O(1) to find median
Space: O(n)

    class MedianFinder {
        PriorityQueue<Integer> max;
        PriorityQueue<Integer> min;
        public MedianFinder() {
            Comparator<Integer> minh = new Comparator<Integer>() {
                public int compare(Integer a, Integer b) {
                    if(a>b)
                        return 1;
                    else if(a<b)
                        return -1;
                    else
                        return 0;
                }
            };
            Comparator<Integer> maxh = new Comparator<Integer>() {
                public int compare(Integer a, Integer b) {
                    if(a>b)
                        return -1;
                    else if(a<b)
                        return 1;
                    else
                        return 0;
                }
            };

            max = new PriorityQueue<>(maxh);
            min = new PriorityQueue<>(minh);
        }
        public void addNum(int num) {
            if(max.size() == 0 && min.size() == 0)
                max.add(num);
            else {
                if(num > max.peek())
                    min.add(num);
                else
                    max.add(num);
            }
            if(Math.abs(max.size() - min.size()) > 1) {
                if(max.size() > min.size())
                    min.add(max.remove());
                else
                    max.add(min.remove());
            }
        }
        public double findMedian() {
            if(max.size() > min.size())
                return (double) max.peek();
            else if(min.size() > max.size())
                return (double) min.peek();
            else
                return ( (double) max.peek() + (double) min.peek() )/2;
        }
    }

## 2Pointer

## Recursion

## Tree

### Path sum 1

    private boolean preorder(TreeNode root, int sum) {
        if(sum == 0 && root.left == null && root.right == null)
            return true;
        if(sum != 0 && root.left == null && root.right == null)
            return false;
        boolean left = false; boolean right = false;
        if(root.left != null)
            left = preorder(root.left, sum-root.left.val);
        if(root.right != null)
            right = preorder(root.right, sum-root.right.val);
        return left || right;
    }
    
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
        return preorder(root, sum-root.val);
    }
    
### Path sum 2

    void backtrack(List<List<Integer>> res, List<Integer> path, TreeNode root, int sum, int current) {
        if(current+root.val == sum && root.left==null && root.right == null) {
            List l = new ArrayList<>(path);
            l.add(root.val);
            res.add(l);
            return;
        }
        else if(current+root.val != sum && root.left==null && root.right == null)
            return;
        
        current+= root.val;
        
        // make change
        path.add(root.val);
        
        //recurse
        if(root.left != null)
            backtrack(res, path, root.left, sum, current);
        if(root.right != null)
            backtrack(res, path, root.right, sum, current);
        
        //undo change
        path.remove(path.size()-1);
        
        
    }
    
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null) return res;
        List<Integer> path = new ArrayList<>();
        backtrack(res, path, root, sum, 0);
        return res;
    }

