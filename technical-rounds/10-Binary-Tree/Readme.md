
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

## 1. Search in BST

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

## 2. Tree Traversal

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

# Sample Problems

## Traversal based:

### 1. Given preorder and inorder construct binary tree

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
	    
### 2. Given postorder and inorder construct binary tree

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

### 3. Find next element (inorder successor) in Binary Tree

### 4. Second minimum node in Binary Tree



## Recursion Based:


