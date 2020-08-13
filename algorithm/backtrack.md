# 回溯法（暴力递归）

> 要把大问题拆成小问题，大问题做了某种决定=>用参数传给小问题=>小问题继续执行
是 DFS 深度搜索一种，一般用于全排列，穷尽所有可能，遍历的过程实际上是一个决策树的遍历过程。
时间复杂度一般 O(N!)，它不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高。

## 模版

```python
res = []
def backtrack(选择列表，路径)：
	if base case:
		res.append(path)
		return
	for 选择 in 选择列表:
		复制选择列表
		在复制表中做选择
		backtrack(复制的选择列表，路径)
```

## [汉诺塔问题](https://leetcode-cn.com/problems/hanota-lcci/)

使用递归move函数。将A通过B移动到C，base case就是n=1时候直接移动过去。n>1时候就是n-1先移动到B。重复
```python
def hanota(self, A: List[int], B: List[int], C: List[int]) -> None:
        def move(n, A, B, C):
            if n == 1:
                C.append(A.pop())
                return
            else:
                move(n-1, A, C, B)
                C.append(A.pop())
                move(n-1, B, A, C)
        move(len(A), A, B, C)
```

## [子集](https://leetcode-cn.com/problems/subsets/)

```python
def subsets(self, nums: List[int]) -> List[List[int]]:
	result = []
	def process(nums, i, res):
		if i == len(nums):
			result.append(res)
			return 
		process(nums, i+1, res+[nums[i]])
		process(nums, i+1, res)
	process(nums, 0, [])
	return result
```

## [子集 II](https://leetcode-cn.com/problems/subsets-ii/)
sort排序大法太简单，时间复杂度太高。用去重，看题解
```python
def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
	result = []
	def process(nums, i, res):
		if i == len(nums):
			res.sort()
			if res not in result:
				result.append(res)
			return 
		process(nums, i+1, res+[nums[i]])
		process(nums, i+1, res)
	process(nums, 0, [])
	return result
```

## [全排列](https://leetcode-cn.com/problems/permutations/)

47分， 空间复杂度可以优化

```python
def permute(self, nums: List[int]) -> List[List[int]]:
	def process(nums, path, res):
		if not nums: 
			res.append(path)
			return 
		# picks = set()
		for i in range(len(nums)):
			nextpath = path.copy()
			nextpath += [nums[i]]
			nextnums = nums.copy()
			nextnums.pop(i)
			process(nextnums, nextpath, res)
	res = []
	process(nums, [], res)
	return res
'''

## [全排列 II](https://leetcode-cn.com/problems/permutations-ii/)
空间复杂度可以优化
```python
def permuteUnique(self, nums: List[int]) -> List[List[int]]:
	def process(nums, path, res):
		if not nums: 
			res.append(path)
			return 
		picks = set()
		for i in range(len(nums)):
			if nums[i] not in picks:
				picks.add(nums[i])
				nextpath = path.copy()
				nextpath += [nums[i]]
				nextnums = nums.copy()
				nextnums.pop(i)
				process(nextnums, nextpath, res)
	res = []
	process(nums, [], res)
	return res
```