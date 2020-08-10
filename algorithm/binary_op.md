# 二进制

## 常见二进制操作

^ 是异或运算，有且仅有一个为1时，才等于1。

#### 基本操作
a=0^a=a^0

0=a^a

由上面两个推导出：a=a^b^b

#### 交换两个数
a=a^b

b=a^b

a=a^b

#### 移除最后一个 1
a=n&(n-1)

#### 获取最后一个 1
diff=n&(~n+1)


## [只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
所有数求异或运，只出现一次结果就是他，两个一样的数字异或得0
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        res = 0
        for num in nums:
            res ^= num
        return res
```

## [只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)
两个位，第一位出现两次第二位为1，第一位清零。当第二位为1还出现的时候清零第一位。最后得到第一位为1的就是最终结果。
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        one, two = 0,0
        for num in nums:
            one = ~two & (one^num)
            two = ~one & (two^num)
        return one
```

## [只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)
先取两个数的异或，至少有一位是1。取出最后边的1，然后和这位所有是1的异或，得出其中一个数。两个数的异或结果在和其中一个求异或得到另外一个。
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> List[int]:
        e0,e1 = 0,0
        for num in nums:
            e0 ^= num
        rightone = e0 & (~e0 + 1)
        for num in nums:
            if (num&rightone) != 0:
                e1 ^= num
        return [e1, e0^e1]
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
