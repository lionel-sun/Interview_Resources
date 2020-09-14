# 树

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

	res, q = [], [root]

	while q:
		cur = q.pop()
		res.append(cur.val)
		if cur.left: q.append(cur.left)
		if cur.right: q.append(cur.right)
	
	return res[::-1]
```

## [二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)


- [二叉树的层次遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/) 

> 下面的代码将res的append改成从左侧添加，insert(0, level)
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
## [二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

思路：如果只是为了完成任务则找到最后一个叶子节点满足插入条件即可。但此题深挖可以涉及到如何插入并维持平衡二叉搜索树的问题，并不适合初学者。
```python
def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
    if not root: return TreeNode(val)
    node = root
    while True:
        if val > node.val:
            if not node.right:
                node.right = TreeNode(val)
                return root
            else:
                node = node.right
        else:
            if not node.left:
                node.left = TreeNode(val)
                return root
            else:
                node = node.left
```

## [二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)
两个条件判断：1，之前出现过叶子节点，但左右子树还有值。2，右子树有值，左没有。使用队列先进先出

```python
def isCompleteTree(self, root: TreeNode) -> bool:
	if not root: return True
	stack, leaf = [root], False
	while stack:
		node = stack.pop(0)
		if leaf and not (not node.left and not node.right):
			return False
		elif not node.left and node.right:
			return False
		if node.left:
			stack.append(node.left)
		if node.right:
			stack.append(node.right)
		if not node.left or not node.right:
			leaf = True
	return True
```

## [二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)
递归套路，深度优先DFS
```python
def serialize(self, root):
	def dfs(node):
		if node:
			vals.append(str(node.val))
			dfs(node.left)
			dfs(node.right)
		else:
			vals.append("null")

	vals = []
	dfs(root)
	return ",".join(vals)

def deserialize(self, data):
	def dfs(vals):
		v = vals.pop(0)
		if v == "null":
			return None
		node = TreeNode(int(v))
		node.left = dfs(vals)
		node.right = dfs(vals)
		return node
	vals = data.split(",")
	return dfs(vals)
```
非递归套路，BFS广度优先遍历
```python
def serialize(self, root):
	res, queue = [], deque([root])
	while queue:
		node = queue.popleft()
		if node:
			queue.append(node.left)
			queue.append(node.right)
			res.append(str(node.val))
		else:
			res.append("null")
	return ",".join(res)

def deserialize(self, data):
	vals = data.split(",")
	idx = 0
	val = vals[idx]
	if val == "null":
		return None
	root = TreeNode(int(val))
	queue = deque([root])
	while queue:
		node = queue.popleft()
		idx += 1
		val = vals[idx]
		if val != "null":
			node.left = TreeNode(int(val))
			queue.append(node.left)
		idx += 1
		val = vals[idx]
		if val != "null":
			node.right = TreeNode(int(val))
			queue.append(node.right)
	return root
```

## [满二叉树]()
判断节点左右子树是否都有，DFS深度递归。如果不存在就返回False
## [二叉树后继和前驱](https://www.jianshu.com/p/ada8570401f6)

## [折纸问题](https://www.nowcoder.com/questionTerminal/430180b66a7547e1963b69b1d0efbd3c)
中序遍历，父节点左子树是下，右子树是上
```python
def foldPaper(self, n):
	res = []
	def midTraversal(n, postion):
		if not n:
			return
		midTraversal(n - 1, "left")
		if postion == "left":
			res.append("down")
		else:
			res.append("up")
		midTraversal(n - 1, "right")
	midTraversal(n,"left")
	return res
```

## 二叉搜索树

## 前缀树

前缀树的3个基本性质：入门07 code01

1.根节点不包含字符，除根节点外每一个节点都只包含一个字符。

2.从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。

3.每个节点的所有子节点包含的字符都不相同。

```python
from collections import defaultdict
class TireTree:
	# pass多少个字符经过这个点，end多少个在这个节点停止的单词。
	pass, end = 0,0
	# next = defaultdict(int)
	next = [None] * 26
	def __init__(self):
		
```
