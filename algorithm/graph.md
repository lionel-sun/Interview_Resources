# 图相关算法

## 图的定义

```python
Class Graph:
	nodes = set()
	edges = set()
	
	def __init__(self, Nodes, Edges):
		nodes = set(Nodes)
		edges = set(Edges)

Class Node:
	value, in, out = None, None, None
	nextNode = []
	nextEdge = []
	
	def __init__(self, Node, Edge):
		self.nextNode = Node
		self.nextEdge = Edge
		
```


## 广度遍历

第一种使用队列，每层先压进去，然后弹出时候打印数值并且讲链接的没进过队列的压进去。
第二种是分层遍历，可以打印是第几层

```python
from collestions import deque

def BFS(node):
	visit = set()
	q = deque()
	q.append(node)
	visit.add(node)
	while q:
		v = q.popleft()
		print(v.value)
		for i in v.nextNode:
			if i not in visit:
				visit.add(i)
				q.append(i)
	return	

def BFS(node):
	visit = set()
	q = deque()
	q.append(node)
	visit.add(node)
	level = 0
	while q:
		level+=1
		n = len(q)
		for _ in range(n):
			v = q.popleft()
			print(level)
			print(v)
			for i in v.nextNode:
				if i not in visit:
					visit.add(i)
					q.append(i)
	return
```

## 深度遍历

DFS使用栈，先压进去，弹出来在压进去。

```python
def DFS(node):
	stack = []
	visit = set()
	stack.append(node)
	visit.add(node)
	print(node.value)
	whiel stack:
		cur = stack.pop()
		for next in cur.nextNode:
			if next not in visit:
				stack.append(cur)
				stack.append(next)
				print(next.value)
				visit.add(next)
				break
	return 
```

## 拓扑排序

要求有入度为0的节点，并且没有环

```python
from collestions import deque,defaultdict

def sortedtopology(graph):
	inmap = defaultdict(int)
	zeroq = deque()
	for node in graph.nodes:
		inmap[node] = node.in
		if node.in == 0:
			zeroq.append(node)
	
	result = []
	while zeroq:
		cur = zeroq.popleft()
		result.append(cur)
		for next in cur.nextNode:
			inmap[next] -=1
			if inmap[next]==0:
				zeroq.append(next)
	
	return result	
```

## 最小生成树问题

要求无向图。保留边权值最小的树就是最小生成树。

Kruskal算法需要使用并查集。从边入手的，从小到大考察每一条边。使用并查集来实现。
- [并查集]()
```python
Class solution：
	parent = list(range(N + 1))
	rank = [1] * (N + 1)
	
	def find(x):
		if parent[parent[x]] != parent[x]:
			parent[x] = find(parent[x])
		return parent[x]
	
	def union(self, x, y):
		px, py = self.find(x), self.find(y)
		if px == py:
			return False
		
		if rank[px] > rank[py]:
			parent[py] = px
		elif rank[px] < rank[py]:
			parent[px] = py
		else:
			parent[px] = py
			rank[py] += 1
		
		return True
	
	

	def kruskal(self, graph):
		# x和y之间权重是w
		edges = sorted(graph.edges)
		for x, y, w in edges:
			if self.union(x,y):
			return w

```

Prim使用优先队列，从任意点开始，解锁相邻的边，从中选取最小的边，然后解锁点继续加边。
如果边链接的点都在解锁的点集中则放弃继续下一个边。
```python
import heapq

def prim(graph):
	node = graph.nodes.pop()
	min_heap = node.nextEdge
	heapq.heapify(min_heap)
	s = set()
	s.add(node)
	res = []
	while min_heap:
		x, y, w = heapq.heappop(min_heap)
		if y not in s:
			s.add(y)
			for i in y.nextEdge:
				heapq.heappush(min_heap, i)
			res.append(())
	return res
```

## 最短路径算法

Dijkstra 算法，边权值没有负数，有向无向都可以。从头结点开始，相邻节点依次加入，选出最小的点加入点集合，继续从这个点开始重复。

```python
import heapq
from collestions import defaultdict

def dijkstra(graph):
	head = graph.nodes[0]
	s = set()
	min_heap=[(0,head)]
	d = {}
	d[head] = 0
	while min_heap:
		dis, node = heapq.heappop(min_heap)
		if node not in s:
			s.add(node)
			for i in node.nextNode:
				heapq.heappush(min_heap,(i.node, dis+i.value)
				if i.node not in d:
					d[i.node] = i.value
				else:
					d[i.node] = min(d[i.node], dis+i.value)
	return d
```

## []()

```python
```

## []()

```python
```
