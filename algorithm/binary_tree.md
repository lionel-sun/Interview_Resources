# 二叉树

## [二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
递归
```python
def preorderTraversal(self, root: TreeNode) -> List[int]:
    res = []
    def dfs(root):
        nonlocal res
        if not root: return None
        res.append(root.val)
        dfs(root.left)
        dfs(root.right)

    dfs(root)
    return res
```
非递归
```python
def preorderTraversal(self, root: TreeNode) -> List[int]:
    if not root: return []
    preoder, stack = [], [root]
    while stack:
        cur = stack.pop()
        preoder.append(cur.val)
        if cur.right:
            stack.append(cur.right)
        if cur.left:
            stack.append(cur.left)
    return preoder
```

## [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
递归
```python
def inorderTraversal(self, root: TreeNode) -> List[int]:
    res = []
    def inorder(root):
        nonlocal res
        if not root: return None
        inorder(root.left)
        res.append(root.val)
        inorder(root.right)
    inorder(root)
    return res
```
非递归
```python
def inorderTraversal(self, root: TreeNode) -> List[int]:
    if not root: return []
    cur, stack, res = root, [], []
    while cur or stack:
        while cur:
            stack.append(cur)
            cur = cur.left
        tmp = stack.pop()
        res.append(tmp.val)
        cur = tmp.right
    return res
```

## [二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)
递归
```python
def postorderTraversal(self, root: TreeNode) -> List[int]:
    if not root: return []
    res = []
    def post(root):
        if not root: return None
        nonlocal res
        post(root.left)
        post(root.right)
        res.append(root.val)
    post(root)
    return res
```
非递归
```python
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if not root: return []
        cur, stack, res = root, [], []
        while cur or stack:
            while cur:
                res.append(cur.val)
                stack.append(cur)
                cur = cur.right
            
            tmp = stack.pop()
            cur = tmp.left

        return res[::-1]
```

## [二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
```python
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root: return []

    res, q = [], [root]
    while q:
        level = []
        for i in range(len(q)):
            cur = q.pop(0)
            level.append(cur.val)
            if cur.left:
                q.append(cur.left)
            if cur.right:
                q.append(cur.right)
        res.append(level)
    return res
```

- [力扣详解](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/tu-jie-er-cha-shu-de-si-chong-bian-li-by-z1m/)

## [二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```python
def maxDepth(self, root: TreeNode) -> int:
    if not root:
        return 0

    return 1 + max(self.maxDepth(root.left),self.maxDepth(root.right))
```
非递归
```python
def maxDepth(self, root: TreeNode) -> int:
    depth = 0
    if not root: return depth

    stack = [root]
    while stack:
        depth += 1
        for i in range(len(stack)):
            tmp = stack.pop(0)
            if tmp.left:
                stack.append(tmp.left)
            if tmp.right:
                stack.append(tmp.right)
    return depth
```
## 二叉树的递归套路

递归向他的子书要信息，递归返回的就是这些信息。先列出可能性，然后列出递归需要返回的信息。递归要求左和右是一样的，返回值是一样的。

- [验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
递归套路：返回三个信息：是否是BST，最大值，最小值
```python
def isValidBST(self, root: TreeNode) -> bool:
    if not root: return True

    def process(node):
        isBST = True
        if node.left:
            l_isBST, l_max, l_min = process(node.left)
            isBST = node.val > l_max and isBST
        else:
            l_isBST, l_min = True, node.val
        if node.right:
            r_isBST, r_max, r_min = process(node.right)
            isBST = node.val < r_min and isBST
        else:
            r_isBST, r_max = True, node.val
        return isBST and l_isBST and r_isBST, r_max, l_min

    root_info = process(root)

    return root_info[0]
```
非递归，迭代:从顶点开始判断，依次判断上边界，下边界还有当前值和左右子树当前值的大小，可以提前终止。
```python
def isValidBST(self, root: TreeNode) -> bool:
    if not root: return True
    stack = [[root, float('-inf'), float('inf')]]
    while stack:
        node, low, up = stack.pop()
        if node.left:
            if node.left.val >= node.val or node.left.val <= low:
                return False
            stack.append([node.left, low, node.val])
        if node.right:
            if node.right.val <= node.val or node.right.val >= up:
                return False
            stack.append([node.right, node.val, up])
    return True
```
- [平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
递归套路，子树返回两个信息：1，是否是平衡二叉树。2，子树的深度（可以优化）
```python
def isBalanced(self, root: TreeNode) -> bool:
    if not root: return True

    def process(root):
        if not root: return True, 0
        l_isValid, l_depth = process(root.left)
        r_isValid, r_depth = process(root.right)
        return abs(r_depth-l_depth)<=1 and l_isValid and r_isValid, max(l_depth, r_depth)+1

    return process(root)[0]
```
- [二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
递归套路，返回最大值
```python
def maxPathSum(self, root: TreeNode) -> int:
    maxsum = float('-inf')

    def process(root):
        if not root: return 0
        l_max = process(root.left)
        r_max = process(root.right)
        nonlocal maxsum
        maxsum = max(maxsum, l_max+r_max+root.val)
        return max(0,max(l_max, r_max)+root.val)
    process(root)
    return maxsum
```
- [二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
子树分别返回是否找到q或者p，找不到返回空，当左右子树分别发现qp时候就返回当前节点就是最低公共祖先
```python
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
    if not root: return None
    if root == p or root == q:
        return root
    left = self.lowestCommonAncestor(root.left, p, q)
    right = self.lowestCommonAncestor(root.right, p, q)
    if left and right:
        return root
    elif left:
        return left
    elif right:
        return right
    else:
        return None
```
## BFS（Breath First Search）应用
- [二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root: return []

    stack, res, start_left = [root], [], True
    while stack:
        level = []
        for i in range(len(stack)):
            if start_left:
                tmp = stack.pop(0)
                if tmp.left:
                    stack.append(tmp.left)
                if tmp.right:
                    stack.append(tmp.right)
            else:
                tmp = stack.pop()
                if tmp.right:
                    stack.insert(0, tmp.right)
                if tmp.left:
                    stack.insert(0, tmp.left)
            level.append(tmp.val)
        start_left = not start_left
        res.append(level)
    return res
```
