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

### 116 Populating Next Right Pointers in Each Node
You are given a perfect binary tree where all leaves are on the same level, and every parent has two children.   
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

- Level Order Traversal (store each node in each level)
```
- Time complexity : O(n)
- Space complexity : O(n) since store the entire tree

public Node connect(Node root) {
    if (root == null) return null;
    Queue<Node> queue = new LinkedList();
    queue.offer(root);
        
    while (!queue.isEmpty()) {
        int size = queue.size();
        Node pre = new Node(0);
        for (int i = 0; i < size; i++) {
            Node node = queue.poll();
            if (node.left != null) {         
                pre.next = node.left;
                pre = node.left;
                queue.offer(node.left);
            }
        
            if (node.right != null) {
                pre.next = node.right;
                pre = node.right;
                queue.offer(node.right);
            }
        }
    }
    return root;
}
```

- Store left most node in each level

```
- Time complexity : O(n) since we process each node exactly once.  
- Space complexity : O(1) since we don't make use of any additional data structure for traversing nodes on a particular level like the previous approach does.

public Node connect(Node root) {
        if (root == null) return null;
        Node leftmost = root;
        
        while (leftmost.left != null) {
            Node head = leftmost;
            while (head != null) {
                head.left.next = head.right;
                if (head.next != null) {
                    head.right.next = head.next.left;
                }
                head = head.next;
            }
            
            leftmost = leftmost.left;
        }
        
        return root;
    }

```

### 117 Populating Next Right Pointers in Each Node II
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.  
Initially, all next pointers are set to NULL.

- Level Order Traversal (store each node in each level)  
（同116）

- Store left most node in each level & each child level
```
- Time complexity : O(n)
- Space complexity : O(1)

public Node connect(Node root) {
    if (root == null) return root;
        
    Node firstN = root;
    while (firstN != null) {
        Node head = firstN; 
            
        Node curFirst = new Node(0);
        Node cur = curFirst;
        while (head != null) {
            if (head.left != null) {
                cur.next = head.left;
                cur = cur.next;
            }
            if (head.right != null) {
                cur.next = head.right;
                cur = cur.next;
            }
            head = head.next;
        }
        firstN = curFirst.next;
    }
    
    return root;
}
```

### 236 Lowest Common Ancestor of a Binary Tree
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.  

- Recursive 
```
- Time complexity : O(n)
- Space complexity : O(n) the worst

private TreeNode ans = null;
publich TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    helper(root, p, q);
}
private boolean helper(TreeNode node, TreeNode p, TreeNode q) {
    if (node == nul) return false;
    
    int left = helper(node.left, p, q) ? 1 : 0;
    int right = helper(node.right, p, q) ? 1 : 0;
    int mid = (node == p || node == q) ? 1 : 0;
    
    int val = left + right + mid;
    if (val >= 2) ans = node;
    
    return val > 0;
}
```
```
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) return null;
    if (root == p || root == q) return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
        
    if (left != null && right != null) return root;
    return left == null ? right : left;
}
```

### 314 Binary Tree Vertical Order Traversal
Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

For each node at position (X), its left and right children respectively will be at positions (X-1) and (X+1).

If two nodes are in the same row and column, the order should be from left to right.

- BFS
```
- Time complexity : O(n)
- Space complexity : O(n) the worst

int left = 0;
int right = 0;
List<List<Integer>> lists = new ArrayList();
public List<List<Integer>> verticalOrder(TreeNode root) {
    if (root == null) return lists;
    verticalSize(root, 0);
    for (int i = left; i <= right; i++) {
        lists.add(new ArrayList());
    }
        
    Queue<Pair<TreeNode, Integer>> queue = new LinkedList();
    queue.offer(new Pair<>(root, 0-left));
    while (!queue.isEmpty()) {
        Pair pair = queue.poll();
        TreeNode node = (TreeNode) pair.getKey();
        int idx = (int) pair.getValue();
        lists.get(idx).add(node.val);
        if (node.left != null) {
            queue.offer(new Pair<>(node.left, idx-1));
        }
        if (node.right != null) {
            queue.offer(new Pair<>(node.right, idx+1));
        }
    }
        
    return lists;
}
    
private void verticalSize(TreeNode node, int idx) {
    if (node == null) return;
       
    verticalSize(node.left, idx-1);
    verticalSize(node.right, idx+1);
    
    left = Math.min(left, idx);
    right = Math.max(right, idx);
}

```

### 987 Vertical Order Traversal of a Binary Tree
Given a binary tree, return the vertical order traversal of its nodes values.
For each node at position (X, Y), its left and right children respectively will be at positions (X-1, Y-1) and (X+1, Y-1).

From left to right, top to bottom
Similar to #314, but If two nodes have the same position, then the value of the node that is reported first is the value that is smaller.

- dfs
```
- Time complexity :         
dfs:O(n),       
pq: each loop O(logn) & totally n loop -> O(nlogn)
- Space complexity : pq used to store object of Point -> O(n)

class Point {
    int x,y, val;
    Point(int x, int y, int val) {
        this.x = x;
        this.y = y;
        this.val = val;
    }
}
    
Comparator<Point> cmp = (s1, s2) -> {
    if (s1.x != s2.x) {
        return s1.x - s2.x;
    } else if (s1.y != s2.y) {
        return s2.y - s1.y;
    }
    return s1.val - s2.val;
}; 
    
PriorityQueue<Point> pq = new PriorityQueue<>(cmp);
public List<List<Integer>> verticalTraversal(TreeNode root) {
    List<List<Integer>> lists = new ArrayList();
    if (root == null) return lists;
        
    dfs(0, 0, root);
    int idx = Integer.MAX_VALUE;
    while (!pq.isEmpty()) {
        Point point = pq.poll();
        if (point.x != idx) {
            lists.add(new ArrayList());
            idx = point.x;
        }
        lists.get(lists.size()-1).add(point.val);
    }
    return lists;
}
    
private void dfs(int x, int y, TreeNode node) {
    if (node == null) return;
    pq.offer(new Point(x, y, node.val));
    dfs(x-1, y-1, node.left);
    dfs(x+1, y-1, node.right);  
}    
```

### 200 Number of Islands

- dfs
```
- Time complexity : O(rLen*cLen) 
- Space complexity : wort case O(rLen*cLen)

int result = 0;
public int numIslands(char[][] grid) {
    int rLen = grid.length;
    int cLen = grid[0].length;
    int result = 0;
    for (int i = 0; i < rLen; i++) {
        for (int j = 0; j < cLen; j++) {
            if (grid[i][j] == '1') {
                result += 1;
                dfs(i, j, grid);
            }
        }
    }
        
    return result;
}
private void dfs(int i, int j, char[][] grid) {
    int rLen = grid.length;
    int cLen = grid[0].length;
        
    if (i < 0 || i >= rLen || j < 0 || j >= cLen) return;
    if (grid[i][j] == '0') return;
    grid[i][j] = '0';
    dfs(i+1, j, grid);
    dfs(i, j+1, grid);
    dfs(i-1, j, grid);
    dfs(i, j-1, grid);
}
```

- bfs

```
- Time complexity : O(rLen*cLen) 
- Space complexity : O(min(M,N)) because in worst case where the grid is filled with lands, the size of queue can grow up to min(M,N).

public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
       
    int row = grid.length;
    int col = grid[0].length;
       
    int result = 0;
    Queue<Pair<Integer, Integer>> queue = new LinkedList();
    for (int r = 0; r < row; r++) {
        for (int c = 0; c < col; c++) {
            if (grid[r][c] == '1') {
                grid[r][c] = '0';
                result++;
                Pair pair = new Pair(r, c);
                queue.offer(pair);
                while (!queue.isEmpty()) {
                    Pair p = queue.poll();
                    int i = (int) p.getKey();
                    int j = (int) p.getValue();
                       
                    if (i-1 >= 0  && grid[i-1][j] == '1') {
                        grid[i-1][j] = '0';
                        queue.offer(new Pair(i-1, j));
                    }
                    if (i+1 < row && grid[i+1][j] == '1') {
                        grid[i+1][j] = '0';
                        queue.offer(new Pair(i+1, j));
                    }
                    if (j-1 >= 0 && grid[i][j-1] == '1') {
                        grid[i][j-1] = '0';
                        queue.offer(new Pair(i, j-1));
                    }
                    if (j+1 < col && grid[i][j+1] == '1') {
                        grid[i][j+1] = '0';
                        queue.offer(new Pair(i, j+1));
                    }
                }
            }
        }
    }
       
    return result;
}

```

### 133 Clone Graph
Given a reference of a node in a connected undirected graph.  
Return a deep copy (clone) of the graph.   

Each node in the graph contains a val (int) and a list (List[Node]) of its neighbors.  

- dfs
```
private HashMap<Node, Node> visited = new HashMap();
public Node cloneGraph(Node node) {
    if (node == null) return node;
    if (visited.containsKey(node)) {
        return visited.get(node);
    }
    Node cloneNode = new Node(node.val, new ArrayList());
    visited.put(node, cloneNode);
    for (Node neighbor : node.neighbors) {
        cloneNode.neighbors.add(cloneGraph(neighbor));
    }
        
    return cloneNode;
}
```

### 98 Validate Binary Search Tree
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

- Recursive
```
- Time complexity : O(n) since we visit each node exactly once
- Space complexity : O(n) since we keep up to the entire tree

public boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    return helper(root, Long.MAX_VALUE, Long.MIN_VALUE);
}
    
private boolean helper(TreeNode node, long max, long min) {
    if (node == null) return true;
    if (node.val >= max || node.val <= min) return false;
    return helper(node.left, node.val, min) 
        && helper(node.right, max, node.val);
}
```
