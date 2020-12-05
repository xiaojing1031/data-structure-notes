## BT && BST

### 144. Binary Tree Preorder Traversal
Given the root of a binary tree, return the preorder traversal of its nodes' values.

- Iterative
```
- Time complexity : O(n)
We visit each node exactly once, thus the time complexity is O(n)
- Space complexity : O(n)

public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList();
    if (root == null) return list;
        
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
        
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        list.add(node.val);
        if (node.right != null) stack.push(node.right);
        if (node.left != null) stack.push(node.left);
    }
        
    return list;
}
```

### 94. Binary Tree Inorder Traversal (多思考一下)
Given the root of a binary tree, return the inorder traversal of its nodes' values.

- Iterative
```
- Time complexity : O(n)
- Space complexity : O(n)

public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList();
    if (root == null) return list;
    Stack<TreeNode> stack = new Stack();
        
    TreeNode cur = root;
    while (!stack.isEmpty() || cur != null) {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }   
        cur = stack.pop();
        list.add(cur.val);
        cur = cur.right;
    } 
    
    return list;
}
```

### 145. Binary Tree Postorder Traversal (思考)
Given the root of a binary tree, return the postorder traversal of its nodes' values.

- Iterative
```
- Time complexity : O(n)
- Space complexity : O(n)

public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList();
    if (root == null) return list;
        
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pop();
        list.add(0, cur.val);
        if (cur.left != null) stack.push(cur.left);
        if (cur.right != null) stack.push(cur.right);
    }
    
    return list;
}
```

### 102. Binary Tree Level Order Traversal
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

- Iterative
```
- Time complexity : O(n)
- Space complexity : O(n)

public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> lists = new ArrayList();
    if (root == null) return lists;
        
    Queue<TreeNode> queue = new LinkedList();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> list = new ArrayList();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            list.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        lists.add(list);
    }
    return lists;
}
```

### 104. Maximum Depth of Binary Tree
Given the root of a binary tree, return its maximum depth.

- Recursive
```
- Time complexity : O(n)
- Space complexity : O(n) / O(log(n))

public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    int depthL = 1 + maxDepth(root.left);
    int depthR = 1 + maxDepth(root.right);
    
    return Math.max(depthL, depthR);
}
```

### 112. Path Sum
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum

- 思考：
1. 边界条件：root = null，sum = 0 --> false
2. sum 也可能小于0

- Recursive
```
public boolean hasPathSum(TreeNode root, int sum) {
    if (root == null) return false;
        
    sum -= root.val;
    if (root.left == null && root.right == null) return sum == 0;
    return hasPathSum(root.left, sum) || hasPathSum(root.right, sum);
}
```

### 105. Construct Binary Tree from Preorder and Inorder Traversal
Given preorder and inorder traversal of a tree, construct the binary tree.
Note:
You may assume that duplicates do not exist in the tree.

- 思考：
1. 考虑边缘条件：preorder = [-1], inorder = [-1] -> TreeNode(-1)
2. 考虑inorder 里的right chirld 返回null 的条件？
```
example
preorder = [3,9,1,20,15,7]
inorder = [9,1,3,15,20,7]
```

- Recursive
```
- Time complexity : O(n)
- Space complexity : O(n) since store the entire tree

int preIdx = 0;
int inIdx = 0;
public TreeNode buildTree(int[] preorder, int[] inorder) {
    if (preorder.length == 0 || inorder.length == 0) return null;
    return helper(preorder, inorder, Integer.MAX_VALUE);
}
    
private TreeNode helper(int[] pre, int[] in, int val) {
    if (preIdx == pre.length || in[inIdx] == val) return null;
    TreeNode root = new TreeNode(pre[preIdx]);
        
    preIdx++;
    root.left = helper(pre, in, root.val);
    inIdx++;
    root.right = helper(pre, in, val);
        
    return root;
}
```
