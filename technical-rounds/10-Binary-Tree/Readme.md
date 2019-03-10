
## Tips:

### 1. Use preorder traversal to compute leaf values last. Example: check if a BST is valid, check if two trees are symmetric etc.

For the above problems, we first check if the root matches a given condition (for valid bst min<root<max, for symmetric, roots should be same).

In cases like these use preorder traversal.

### 2. Inorder traversal arranges the tree's nodes in sorted order if it is a BST. This can be useful for situations where you need to get nth smallest/largest element 

When you do inorder traversal in a binary search tree (left<current<right) then an inorder traversal returns the nodes in sorted order. Similarly you can do reverse inorder traversal to get nodes in descending order. 

If you are asked to find the 2nd largest element in a BST then it is the second reverse-inorder element and the second-last inorder element. Similarlt if you are asked to find the second element in a BST, it is the second inorder element and the second-last reverse-inorder element.

Inorder is useful if you want to get the kth smallest or kth largest element in a BST.

### 3. Postorder traversal to get anything that requires you to compute root's valu last. Example: tree height - you need to compute child heights first to compute parent height. So use postorder traversal

Every time you need to get the height use postorder traversal. This reduces the complexity og the getHeight() function to just n. Doing naive recursion will force us to repeatedly compute heights upping complexity to nlogn upto n^2 depending on tree structure.

    private int postorder(TreeNode root) {
        if(root == null) return 0;
        int left = postorder(root.left);
        int right = postorder(root.right);
        return 1+Math.max(left, right);
    }


### 4. Use global variables

Consider using global variables if we are trying to aggregate different results. For example in the question find diameter of tree, it may be either in root or in children - it is not guaranteed to be in root so have a global variable to keep track of things. Similarly in sum root to leaf nodes, we must sum up the various paths at leaves. So keep global variable to keep track of sum.

## Binary Tree

Consider the following tree - it will be used in all examples:

![BST](https://i.imgur.com/p0iWIiA.png)


Code to construct it:

	static class Node {
		int val;
		Node left;
		Node right;
		public Node(int val){
			this.val = val;
			this.left = null;
			this.right = null;
		}
	}
	
	static Node construct() {
		Node root = new Node(10);
		root.left = new Node(-5);
		root.left.left = new Node(-10);
		root.left.right = new Node(0);
		root.left.right.right = new Node(5);
		root.right = new Node(20);
		root.right.left = new Node(15);
		root.right.right = new Node(30);
		root.right.right.right = new Node(35);
		return root;
	}

## Table of Contents

1. [Basics](#Basics)
2. [Level-Order](#Level-Order)
3. [Post-Order](#Post-Order)
4. [In-Order](#In-Order)
5. [Pre-Order](#Pre-Order)
6. [Combination](#Combination)

## Basics

### Search in BST

**Recursive:**

	private static boolean search(Node root, int val) {
		if(root == null)
			return false;
		if(root.val == val)
			return true;
		else if(root.val > val)
			return search(root.left, val);
		else
			return search(root.right, val);
	}

**Iterative:**

	private static boolean search(Node root, int val) {
		while(root != null) {
			if(root.val == val)
				return true;
			else if(root.val > val)
				root = root.left;
			else
				root = root.right;
		}
		return false;
	}

### Tree Traversal

Tree traversals are classified into 2 types: Depth first traversals and Breadth first traversals.

Depth first traversals are:
- Preorder: Visit, Left, Right
- Inorder: Left, Visit, Right
- Postorder: Left, Right, Visit

Breadth first traversals are:
- Level order: Print level by level

### Preorder: 10, -5, -10, 0, 5, 20, 15, 30, 35

**Recursive**

	static void preorder(Node root, List<Integer> arr) {
		if(root == null)
			return;
		arr.add(root.val);
		preorder(root.left, arr);
		preorder(root.right, arr);
	}

**Iterative**

	static void preorder(Node root, List<Integer> arr) {
		Stack<Node> stack = new Stack<>();
		stack.push(root);
		while(!stack.isEmpty()) {
			Node current = stack.pop();
			if(current != null) {
				arr.add(current.val);
				stack.push(current.right);
				stack.push(current.left);
			}
		}
	}

### Inorder: -10, -5, 0, 5, 10, 15, 20, 30, 35

**Recursive**

	static void inorder(Node root, List<Integer> arr) {
		if(root == null)
			return;
		inorder(root.left, arr);
		arr.add(root.val);
		inorder(root.right, arr);
	}

**Iterative**

	static void inorder(Node root) {
		if(root == null)
			return;
		Stack<Node> stack = new Stack<>();
		while(root != null || !stack.isEmpty()) {
			if(root!=null) {
				stack.push(root);
				root = root.left;
			} else {
				root = stack.pop();
				System.out.print(root.val + " ");
				root = root.right;
			}
		}
	}

### Postorder: -10 5 0 -5 15 35 30 20 10

**Recursive**

	static void postorder(Node root) {
		if(root == null)
			return;
		postorder(root.left);
		postorder(root.right);
		System.out.print(root.val+" ");
	}

**Iterative**

	static void postorder(Node root) {
		if(root == null)
			return;
		Stack<Node> stack1 = new Stack<>();
		Stack<Node> stack2 = new Stack<>();
		stack1.push(root);
		while(root != null || !stack1.isEmpty()) {
			root = stack1.pop();
			if(root != null) {
				stack1.push(root.left);
				stack1.push(root.right);
				stack2.push(root);
			}
		}
		while(!stack2.isEmpty()) {
			root = stack2.pop();
			System.out.print(root.val+" ");
		}
	}

### Levelorder: 10 -5 20 -10 0 15 30 5 35 

	static void levelorder(Node root) {
		if(root == null)
			return;
		Queue<Node> queue = new LinkedList<>();
		queue.add(root);
		while(!queue.isEmpty()) {
			root = queue.remove();
			if(root!=null) {
				System.out.print(root.val+" ");
				queue.add(root.left);
				queue.add(root.right);
			}
		}
	}


### Given preorder and inorder construct binary tree

Logic:
- First element of preorder array is root
- We can search the inorder array for root element. All elements to the left of it are the left side of tree, all elements to the right of it are the right side of the tree.
- So we need start of preorder, and both ends of inorder array.

We know the element will be null if:
- element we start at in preorder array > length of preorder array
- inorder array is empty (start > end)

Otherwise we can build as follows:
- Root = inorder_index (inorder_index is find(preorder))
- Left element = preorder+1, inorder_start, inorder_index-1
- Right element = preorder+1+(inorder_index-inorder_start), inorder_index+1, inorder_end

		private TreeNode build(int pre_start, int in_start, int in_end, int[] preorder, int[] inorder) {
			if(pre_start > preorder.length || in_start > in_end)
				return null;
			int root_val = preorder[pre_start];
			int in_index = getIndex(inorder, root_val);

			TreeNode root = new TreeNode(root_val);
			root.left = build(pre_start+1, in_start, in_index-1, preorder, inorder);
			root.right = build(pre_start+(in_index-in_start)+1, in_index+1, in_end, preorder, inorder);
			return root;
		}
	    
### Given postorder and inorder construct binary tree

Logic:

	private TreeNode build(int post_e, int in_s, int in_e, int[] inorder, int[] postorder) {
		if(post_e < 0 || in_s > in_e)
		    return null;

		int root_val = postorder[post_e];
		int in_index = getIndex(inorder, postorder[post_e]);

		TreeNode root = new TreeNode(root_val);
		root.left = build(post_e-(in_e - in_index)-1, in_s, in_index-1, inorder, postorder);
		root.right = build(post_e-1, in_index+1, in_e, inorder, postorder);
		return root;
    }

## Level-Order

### Level order traversal

Logic: Add element to queue. While q is not empty, get the size - this is the number of elements in current level. For i=0 to size, remove item from queue, add its value to list and add its children to the queue.

	class Solution {
	    public List<List<Integer>> levelOrder(TreeNode root) {
		List<List<Integer>> res = new ArrayList<>();
		if(root == null) return res;

		Queue<TreeNode> q = new LinkedList<>();
		q.add(root);

		while(!q.isEmpty()) {
		    int size = q.size();
		    List<Integer> list = new ArrayList<>();
		    for(int i = 0; i<size; i++) {
			TreeNode current = q.remove();
			list.add(current.val);
			if(current.left != null)
			    q.add(current.left);
			if(current.right != null)
			    q.add(current.right);
		    }
		    res.add(list);
		}
		return res;
	    }
	}

### Zig-zag level order traversal

Logic: Have a level variable initialized to 0. Do level order traversal as above. While queue is not empty, get the size and for i=0 to size, pop the queue and add its children. Have a LinkedList and if level%2 is 0, add element to last else if it is 1, add element to front of list. Add the linked List to the result list.

	class Solution {
	    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {

		List<List<Integer>> res = new ArrayList<>();
		if(root == null) return res;

		Queue<TreeNode> q = new LinkedList<>();
		q.add(root);
		int level = 0;

		while(!q.isEmpty()) {
		    int size = q.size();
		    LinkedList<Integer> list = new LinkedList<>();
		    for(int i=0; i<size; i++) {
			TreeNode current = q.remove();
			if(current.left != null)
			    q.add(current.left);
			if(current.right != null)
			    q.add(current.right);
			if(level%2 == 0)
			    list.add(current.val);
			else
			    list.addFirst(current.val);
		    }
		    res.add(list);
		    level++;
		}
		return res;
	    }
	}

### Right side view of binary tree

Logic: Do level order traversal. If node is last node of a given level, it is part of right side view.

	class Solution {
	    public List<Integer> rightSideView(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		if(root == null) return res;

		Queue<TreeNode> q = new LinkedList<>();
		q.add(root);

		while(!q.isEmpty()) {
		    int size = q.size();
		    for(int i=0; i<size; i++) {
                TreeNode current = q.remove();
                if(current.left != null)
                    q.add(current.left);
                if(current.right != null)
                    q.add(current.right);
                if(i==size-1)
                    res.add(current.val);
		    }
		}
		return res;
	    }
	}


### Boundary of binary tree in anticlockwise order

Logic: Boundary consists of left view, right view and leaves. To get the correct order do: left view of left subtree ignoring all leaves, get leaves using preorder, right view of right subtree ignoring all leaves

**Needs correction**

	class Solution {

	    private void leftview(TreeNode root, List<Integer> res) {
		if(root == null) return;
		Queue<TreeNode> q = new LinkedList<>();
		q.add(root);

		while(!q.isEmpty()) {
		    int size = q.size();
		    for(int i=0; i< size; i++) {
			TreeNode current = q.remove();
			if(current.left != null)
			    q.add(current.left);
			if(current.right != null)
			    q.add(current.right);
			if(i==0 && (current.left != null || current.right != null))
			    res.add(current.val);
		    }
		}
	    }

	    private void rightview(TreeNode root, List<Integer> res) {
		if(root == null) return;
		Queue<TreeNode> q = new LinkedList<>();
		q.add(root);

		List<Integer> li = new ArrayList<>();
		while(!q.isEmpty()) {
		    int size = q.size();
		    for(int i=0; i< size; i++) {
			TreeNode current = q.remove();
			if(current.left != null)
			    q.add(current.left);
			if(current.right != null)
			    q.add(current.right);
			if(i==size-1 && (current.left != null || current.right != null))
			    li.add(current.val);
		    }
		}
		for(int i=li.size()-1; i>=0; i--)
		    res.add(li.get(i));
	    }

	    private void leafview(TreeNode root, List<Integer> res) {
		if(root.left == null && root.right == null) return;
		Stack<TreeNode> stack = new Stack<>();
		stack.push(root);
		List<Integer> li = new ArrayList<>();
		while(!stack.isEmpty()) {
		    TreeNode current = stack.pop();
		    if(current.left == null && current.right == null)
			li.add(current.val);

		    if(current.right != null)
			stack.push(current.right);
		    if(current.left != null)
			stack.push(current.left);

		}
		for(int i=0; i<li.size(); i++)
		    res.add(li.get(i));
	    }

	    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		if(root == null) return res;
		res.add(root.val);
		leftview(root.left, res);
		leafview(root, res);
		rightview(root.right, res);
		return res;
	    }
	}

### Average of levels in Binary Tree

    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        if(root == null) return res;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while(!queue.isEmpty()) {
            int size = queue.size();
            double sum = 0;
            for(int i=0; i<size; i++) {
                TreeNode current = queue.remove();
                sum+=current.val;
                if(current.left!=null)
                    queue.add(current.left);
                if(current.right!=null)
                    queue.add(current.right);
            }
            res.add( sum/size );
        }
        return res;
    }


## Post-Order

### Find leaves of binary tree

Given a binary tree, collect a tree's leaves level by level. So take first layer of leaves and put them in a list. Then take next layer and put them in another list and so on. Store it in a list of lists.

Logic: The height of leaf nodes = 0. The height of next set of leaf nodes = 1 and so on. Do postorder traversal to get height. If a list exists in list of lists for height of node, add value to list. Else crate list for that height and add value to it.

	class Solution {

	    private int postorder(TreeNode root, List<List<Integer>> res) {
			if(root == null)
				return 0;
			int left = postorder(root.left, res);
			int right = postorder(root.right, res);
			int height = Math.max(left, right);
			if(height >= res.size())
				res.add(new ArrayList<Integer>());
			res.get(height).add(root.val);
			return 1+height;
	    }

	    public List<List<Integer>> findLeaves(TreeNode root) {
			List<List<Integer>> res = new ArrayList<>();
			postorder(root, res);
			return res;
	    }
	}


### Find diameter of binary tree

Logic: Diameter of the tree is max(f(root), f(root.left), f(root.right)). f(root) = height of left subtree+ height of right subtree.

	class Solution {
	    private int getHeight(TreeNode root) {
		if(root == null) return 0;
		else return 1+Math.max(getHeight(root.left), getHeight(root.right));
	    }

	    public int diameterOfBinaryTree(TreeNode root) {
		if(root == null)
		    return 0;
		int h = getHeight(root.right) + getHeight(root.left);
		return Math.max(h, Math.max(diameterOfBinaryTree(root.left), diameterOfBinaryTree(root.right)));
	    }
	}

Complexity: O(n logn) because height takes O(logn) and we get height for each node in tree.

Optimization: Use postorder traversal method to incrementally compute height.

	class Solution {
	    int global_diameter = 0;
	    private int postorder(TreeNode root) {
		if(root == null)
		    return 0;
		int left = postorder(root.left);
		int right = postorder(root.right);
		int local_diameter = left+right;
		global_diameter = Math.max(global_diameter, local_diameter);
		return 1+Math.max(left,right);
	    }

	    public int diameterOfBinaryTree(TreeNode root) {
		postorder(root);
		return global_diameter;
	    }
	}

### Check if Binary Tree is balanced

Logic: It is balanced if height difference between subtrees is more than 1. Since height is involved, use the postorder method. Have global boolean and each postorder step, check condition for boolean to be false.

	class Solution {
	    boolean balanced = true;
	    private int postorder(TreeNode root) {
		if(root == null || balanced == false)
		    return 0;
		int left = postorder(root.left);
		int right = postorder(root.right);
		if(Math.abs(left-right) >=2) {
		    balanced = false;
		    return 0;
		}
		return 1+Math.max(left,right);
	    }

	    public boolean isBalanced(TreeNode root) {
		postorder(root);
		return balanced;
	    }
	}

## In-Order

### Second minimum node in Binary Search Tree with duplicates

If we did not have duplicates we could take advantage of BST structure and do:

	if(root.left == null)
		return getleftmost(root.right)
	TreeNode parent = null;
	while(root.left != null) {
		parent = root;
		root = root.left;
	}
	if(root.right != null)
		return getleftmost(root.right);
	else
		return parent;

Logic: Set previous value = some impossible value and k=2. Do inorder traversal if value is different from prev, decrement k and update previous value. If k=0 return value otherwise continue. At end of loop return -1.

	class Solution {

	    public int findSecondMinimumValue(TreeNode root) {
		int k = 2;
		long prev = Long.MAX_VALUE;
		Stack<TreeNode> stack = new Stack<>();
		while(root!=null || !stack.isEmpty()) {
		    while(root!=null) {
			stack.push(root);
			root = root.left;
		    }
		    root = stack.pop();
		    if(root.val != prev) {
			prev = root.val;
			k--;
			if(k==0)
			    return (int) prev;
		    }
		    root = root.right;
		}
		return -1;
	    }
	}

### Find next element (inorder successor) in Binary Tree

Two ways to do this: 1. Inorder traversal and 2. Take advantage of the structure of BST by returning next greater element.

1. Inorder traversal: Just do regular inorder travresal and return next. Time: n, space: h

		class Solution {
			public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
				Stack<TreeNode> stack = new Stack<>();
				boolean found = false;
				while(root!=null || !stack.isEmpty()) {
					while(root != null) {
						stack.push(root);
						root = root.left;
					}
					root = stack.pop();
					if(found)
						return root;
					if(root == p)
						found = true;
					root = root.right;
				}
				return null;
			}
		}

2. Taking advantage of the structure:

Keep track of smallest ancestor. Find p. If p.right is not null find leftmost of right subtree otherwise return the parent.
Time: h, space: 1

	class Solution {

	    private TreeNode getLeftmost(TreeNode root) {
		if(root == null) return root;
		while(root.left != null)
		    root = root.left;
		return root;
	    }

	    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
		TreeNode parent = null;
		while(root!=p) {
		    if(root.val > p.val) {
			parent = root;
			root = root.left;
		    } else
			root = root.right;
		}
		if(root.right == null)
		    return parent;
		return getLeftmost(root.right);
	    }
	}


## Pre-Order

### Path Sum: Return true if path from root to any leaf sums up to target

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

### Sum root to leaf numbers

	class Solution {
		int sum = 0;
		private void preorder(TreeNode root, int val) {
			if(root.left == null && root.right == null) {
				sum+= val;
				return;
			}

			if(root.left!=null) {
				preorder(root.left, val*10+ root.left.val);
			}
			if(root.right!=null) {
				preorder(root.right, val*10+ root.right.val);
			}
		}

		public int sumNumbers(TreeNode root) {
			if(root == null) return 0;
			preorder(root, root.val);
			return sum;
		}
	}

## Combination

### Path sum 3: Return all paths in tree that sum up to target. Path can start at any node and end at any node


	class Solution {

	    private int getpaths(TreeNode root, int sum) {
		if(root == null)
		    return 0;
		if(root.val==sum)
		    return 1 + getpaths(root.left, sum-root.val) + getpaths(root.right, sum-root.val);
		else
		    return 0 + getpaths(root.left, sum-root.val) + getpaths(root.right, sum-root.val);
	    }

	    public int pathSum(TreeNode root, int sum) {
		if(root == null) return 0;
		return getpaths(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
	    }
	}

### Longest Univalue path

	class Solution {
	    private int postorder(TreeNode root, int val) {
		if(root == null || root.val != val)
		    return 0;
		int left = postorder(root.left, val);
		int right = postorder(root.right, val);
		return 1+Math.max(left, right);
	    }

	    public int longestUnivaluePath(TreeNode root) {
		if(root == null) return 0;
		int path = postorder(root.left, root.val) + postorder(root.right, root.val);
		return Math.max(path, Math.max(longestUnivaluePath(root.left), longestUnivaluePath(root.right)));
	    }
	}
