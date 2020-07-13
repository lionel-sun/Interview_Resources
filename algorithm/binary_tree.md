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
