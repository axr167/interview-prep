
### 1. Given graph find if there is path between 2 nodes

Either use BFS or DFS

### 2. Given sorted list create binary tree with min height

Leetcode 108.

Logic:
- Mid = root
- root.left = a(0...mid-1); root.right = a(mid+1...hi)
- Do while lo<=hi

Code:

    
    private TreeNode create(int[] nums, int lo, int hi) {
        if (lo>hi)
            return null;
        int mid = (lo+hi)/2;
        TreeNode t = new TreeNode(nums[mid]);
        t.left = create(nums, lo, mid-1);
        t.right = create(nums, mid+1, hi);
        return t;
    }
    
    public TreeNode sortedArrayToBST(int[] nums) {
        TreeNode t = create(nums, 0, nums.length-1);
        return t;
    }

### 3. Given binary tree create list of lists containing nodes according to depth

Level order traversal (102) leetcode

Logic: 

- Create list of lists res. Then call helper function passing res, root and level of root (0)
- In helper, check if res has a list at given index. If not, create list and add to res. Add value to the list at given level
- Call helper for children incrementing current level

Code:

    private void traverse(List<List<Integer>> res, TreeNode root, int level) {
        if (root == null)
            return;
        if(level >= res.size()) {
            List<Integer> li = new ArrayList<>();
            res.add(level, li);
        }
        res.get(level).add(root.val);
        traverse(res, root.left, level+1);
        traverse(res, root.right, level+1);
    }
    
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        traverse(res, root, 0);
        return res;
    }

### 4. Check if binary tree is balanced

Method 1: Get height of left, right. If it is more than 1, return false else return true. This is O(n logn)

    private int getHeight(TreeNode root) {
        if(root == null)
            return -1;
        else
            return Math.max(1+getHeight(root.left), 1+getHeight(root.right));
    }
    private boolean check(TreeNode root) {
        if(root == null)
            return true;
        if( Math.abs(getHeight(root.left) - getHeight(root.right)) > 1 )
            return false;
        else
            return check(root.left) && check(root.right);
        
    }
    
Method 2: If root = null height = -1. Get height of left and right. If either left or right is unbalanced, return -2. Otherwise check if current node is unbalanced. If it is, return -2 otherwise return height. This is O(n) because each node's height is checked only once.

    private int getHeight(TreeNode root) {
        if(root == null)
            return -1;
        int l = getHeight(root.left);
        int r = getHeight(root.right);
        if(l==-2||r==-2)
            return -2;
        if(Math.abs(l-r) > 1)
            return -2;
        else
            return (1+ Math.max(l,r));
    }
    
    public boolean isBalanced(TreeNode root) {
        if(getHeight(root) == -2)
            return false;
        return true;
    }
