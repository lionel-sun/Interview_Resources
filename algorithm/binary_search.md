# 二分法

## 二分搜索模版

给一个有序数组和目标值，找第一次/最后一次/任何一次出现的索引，如果没有出现返回-1

模板四点要素：
- 1、初始化：start=0、end=len-1
- 2、循环退出条件：start + 1 < end
- 3、比较中点和目标值：A[mid] ==、 <、> target
- 4、判断最后两个元素是否符合：A[start]、A[end] ? target

时间复杂度 O(logn)，使用场景一般是有序数组的查找

## [二分查找](https://leetcode-cn.com/problems/binary-search/)

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)-1

        while l+1 <r:
            mid = l + (r-l)//2
            if nums[mid]<target:
                l = mid
            else:
                r = mid
        if nums[l] == target:
            return l
        elif nums[r] == target:
            return r
        else:
            return -1
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