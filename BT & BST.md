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
