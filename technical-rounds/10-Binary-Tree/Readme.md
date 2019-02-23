
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
