# 动态规划

## 贪心算法

### 会议室安排问题

输入项目是开始时间和结束时间，对列表排序根据结束时间，从结束时间最早的开始安排，每次更新时间点和可以安排的数量
```python
def arrange(programs, timepoint):
	res = 0
	# programs = [[start, end]]
	programs.sort(key = lambda x : x[1])
	
	for i in programs:
		if timepoint <= i[0]:
			res+=1
			timepoint = i[1]
	return res
```

### 字符串

一组字符串，组成一个字符串要求字典序最小（字典序：长度一样按数字比，长度不一样的情况下短的后面加0补全然后比。）

假设ab拼接小于ba，则a在b前面，用这个对列表全排列。使用比较器。
```python
from functools import cmp_to_key
def combinestr(l):
	def cmp(str1, str2):
		if str1+str2 > str2+str2:
			return 1
		else:
			return -1
	l.sort(key=cmp_to_key(cmp))
	print("".join(l))
```

### 切金条问题

给一个数组，求完整金条切成数组大小最小花费，每次切花费是它本身。
使用小跟堆存放数组，每次合并最小的两个，合并完放进去。这个过程逆过来就是切割过程。
```python
import heapq
def lessmoneysplitgold(l):
	heapq.heapqify(l)
	res = 0
	while len(l)>1:
		a1 = heapq.heappop(l)
		a2 = heaqp.heappop(l)
		sum = a1+a2
		res+=sum
		heap.heappush(l, sum)
	return res
```

### IPO项目投资

两个数组cost和profit是成本和利润。做多做k个项目，m初始资金。输出最后获得最大钱数

```python
import heapq
def ipo(costs, profits, k, m):
	mincosthp = []
	maxprofithp = []
	for i in range(len(costs)):
		heapq.heappush(mincosthp,[costs[i],profits[i]])
	for i in range(k):
		while mincosthp and mincosthp[0] < m:
			pro = heap.heappop(mincosthp)
			heap.heappush(maxprofithp,[-pro[1],pro[0]])
		if not maxprofithp: return m
		do = heap.heappop(maxprofithp)
		m -= do[0]
	return m
```





















