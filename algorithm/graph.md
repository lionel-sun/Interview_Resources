# 图相关算法

## 图的定义

```python
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

```python

```

## []()

```python
```