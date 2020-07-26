# 排序算法

> 稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面。
> 不稳定：如果a原本在b的前面，而a=b，排序之后 a 可能会出现在 b 的后面。

## 测试排序主函数
```python
arr = []
for i in range(random.randint(10, 20)):
    arr.append(random.randint(-100, 100))
listarr = arr.copy()
print(listarr)
print("   bubble sort: ", bubbleSort(listarr))
listarr = arr.copy()
# print(listarr)
print("selection sort: ", selectionSort(listarr))
listarr = arr.copy()
print("insertion sort: ", insertionSort(listarr))
```
## [冒泡排序]()
稳定
```python
def bubbleSort(arr):
    for j in range(len(arr)):
        for i in range(len(arr)-1):
            if arr[i]>arr[i+1]:
                arr[i], arr[i+1] = arr[i+1],arr[i]
            i+=1
        j+=1

    return arr
```

## [选择排序]()
不稳定
```python
def selectionSort(arr):
    for i in range(len(arr)):
        minindex = i
        for j in range(i, len(arr)):
            if arr[j] < arr[minindex]:
                minindex = j
        arr[i], arr[minindex] = arr[minindex], arr[i]
    return arr
```

## [插入排序]()
稳定
```python
def insertionSort(arr):
    for i in range(1, len(arr)):
        pre = i-1
        cur = arr[i]
        while pre and arr[pre] > cur:
            arr[pre+1] = arr[pre]
            pre -= 1
        arr[pre+1] = cur
    return arr
```

## [归并排序]()
> 递归时间复杂度的master公式
直接分开到最低成，然后两两合并
```python
def merge(A, B):
    C = []
    i, j = 0, 0
    while i < len(A) and j < len(B):
        if A[i] <= B[j]:
            C.append(A[i])
            i += 1
        else:
            C.append(B[j])
            j += 1
    if i < len(A):
        C += A[i:]
    elif j < len(B):
        C += B[j:]
    return C

def mergesort(arr):
    n = len(arr)
    if n < 2:
        return arr
    left = mergesort(arr[:n//2])
    right = mergesort((arr[n//2:]))
    return merge(left, right)
```

## [堆排序]()

```python
def heapify(arr, n, i):
    largest = i
    l = 2 * i + 1  # left = 2*i + 1
    r = 2 * i + 2  # right = 2*i + 2

    if l < n and arr[i] < arr[l]:
        largest = l

    if r < n and arr[largest] < arr[r]:
        largest = r

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]  # 交换

        heapify(arr, n, largest)


def heapSort(arr):
    n = len(arr)
    # Build a maxheap. 从第一个非叶子节点开始
    for i in range(n//2, -1, -1):
        heapify(arr, n, i)
    # 一个个交换元素 将最大值放到最后，然后n-1做为堆排序
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # 交换
        heapify(arr, i, 0)
```

## [快速排序]()
随机选取一个数放在最后做分割，然后返回。
```python
def partition(arr, low, high):
    i = (low - 1)  # 最小元素索引
    ran = random.randint(low,high)
    arr[ran], arr[high] = arr[high], arr[ran]
    pivot = arr[high]

    for j in range(low, high):

        # 当前元素小于或等于 pivot
        if arr[j] <= pivot:
            i = i + 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return (i + 1)


# 快速排序函数
def quickSort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)

        quickSort(arr, low, pi - 1)
        quickSort(arr, pi + 1, high)

```

## [计数排序]()

不基于比较的排序需要有确定范围不能太大。

```python
def countingSort(arr, maxValue):
    bucketLen = maxValue+1
    bucket = [0]*bucketLen
    sortedIndex =0
    arrLen = len(arr)
    for i in range(arrLen):
        if not bucket[arr[i]]:
            bucket[arr[i]]=0
        bucket[arr[i]]+=1
    for j in range(bucketLen):
        while bucket[j]>0:
            arr[sortedIndex] = j
            sortedIndex+=1
            bucket[j]-=1
    return arr
```

## [基数排序](https://www.runoob.com/w3cnote/radix-sort.html)
需要数字有固定范围不能太呆，先从最小位分别进入桶在出来到最大位，最后依次出来就是最终排序
```python

```

## []()

```python

```

## []()

```python

```

## []()

```python

```

## []()

```python

```
