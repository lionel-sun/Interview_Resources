# 排序算法

> 稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面。
> 不稳定：如果a原本在b的前面，而a=b，排序之后 a 可能会出现在 b 的后面。

## 测试排序主函数
```python
arr = []
for i in range(random.randint(1, 10)):
    arr.append(random.randint(-100, 100))
print(arr)
print("bubble sort: ", bubbleSort(arr))
print("selection sort: ", selectionSort(arr))
print("insertion sort: ", insertionSort(arr))
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

```

## [快速排序]()

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

## []()

```python

```

## []()

```python

```
