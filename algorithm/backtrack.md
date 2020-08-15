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

## [解码方法](https://leetcode-cn.com/problems/decode-ways/description/)

回溯法，超时
```python
def numDecodings(self, s: str) -> int: 
	def process(st, i):
		res = 0
		if i == len(st): return 1
		if st[i] == '0': return 0
		res += process(st, i+1)
		if st[i] == '1' and i +1 <len(st):
			res += process(st, i+2)
		if st[i] == "2" and i +1 <len(st):
			if st[i+1] in "0123456": res += process(st, i+2)
		return res
	return process(s, 0)
```
动态规划
```python
```

## 背包问题

0-1背包问题。有N个货物，values和weights两个数组代表价值和重量,bag表示背包容量。求最大价值

```python
def getMaxValue(v, w, bag):
	def process(v, w, idx, alreadyw, bag):
		if alreadyw > bag: return -1
		if idx == len(w): return 0
		
		p1 = (v, w, idx+1, alreadyw, bag)
		
		p2next = (v, w, idx+1, alreadyw + w[idx], bag)
		p2 = -1
		if p2next != -1:
			p2 = p2next + v[idx]
		return max(p1, p2)
```

## [预测赢家](https://leetcode-cn.com/problems/predict-the-winner/)

回溯法，超时
```python
def PredictTheWinner(self, nums: List[int]) -> bool:
	def f(arr, l, r):
		if l == r:
			return arr[r]
		return max(arr[r]+s(arr ,l, r-1), arr[l]+s(arr, l+1, r))
	
	def s(arr, l, r):
		if l == r:
			return 0
		return min(f(arr, l+1, r), f(arr, l, r-1))
	
	p1 = f(nums, 0, len(nums)-1)
	p2 = s(nums, 0, len(nums)-1)

	return True if p1 >= p2 else False
```

动态规划
```python

```

## [N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)

无法用dp，只能回溯。常规方法，时间复杂度一样，但是常数时间高
```python
def totalNQueens(self, n: int) -> int:
	def process(i, record, n):
		if i == n: return 1
		res = 0
		for j in range(n):
			if isValid(record, i, j):
				record[i] = j;
				res += process(i+1, record, n)
		return res

	def isValid(record, i, j):
		for k in range(i):
			if j == record[k] or abs(record[k]-j) == abs(k-i):
				return False
		return True

	if n < 1: return 0
	record = [None] * n
	return process(0, record, n)
```

使用位运算，java限制32位(int)或者64位(long)，时间复杂度一样，但是常数时间极快
```python
def totalNQueens(self, n: int) -> int:
	def process(limit, colLim, leftDiaLim, rightDiaLim):
		if colLim == limit:
			return 1
		pos = limit & (~( colLim | leftDiaLim | rightDiaLim))
		# pos是1的位置都是可以取的
		res = 0
		while pos != 0:
			mostRgihtone = pos & (~pos + 1)
			pos -= mostRgihtone
			res += process(limit, colLim | mostRgihtone, 
							(leftDiaLim | mostRgihtone) << 1,
							(rightDiaLim | mostRgihtone) >> 1)
		return res
	limit = (1 << n) - 1
	return process(limit, 0,0,0)
```















