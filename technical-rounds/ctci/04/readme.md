
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

### 4. 
