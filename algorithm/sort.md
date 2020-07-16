# 排序算法

> 稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面。
> 不稳定：如果a原本在b的前面，而a=b，排序之后 a 可能会出现在 b 的后面。

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

## []()

```python

```

## []()

```python

```
