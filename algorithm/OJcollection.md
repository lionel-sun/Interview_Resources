# 真题收集

## 广联达

### 第一题

杰夫非常喜欢种草，他自己有一片草地，为了方便起见，我们把这片草地看成一行从左到右，并且第 i 个位置的草的高度是hi。
杰夫在商店中购买了m瓶魔法药剂，每瓶魔法药剂可以让一株草长高x，杰夫希望每次都能有针对性的使用药剂，也就是让当前长得最矮的小草尽量高，现在杰夫想请你告诉他在使用了m瓶魔法药剂之后，最矮的小草在这种情况下最高能到多少。

输入描述
第一行三个整数n, m, x分别表示小草的个数，魔法药剂的个数以及每瓶魔法药剂让小草长高的高度。(1≤n,m,x≤1e5)  第二行n个整数分别表示第i株小草的高度ai。(1≤ai≤1e9)  输出描述
使用了m瓶药剂之后最矮的小草的最高高度。

```python
import sys, heapq
n, m, x = list(map(int, sys.stdin.readline().strip().split()))
cao = list(map(int, sys.stdin.readline().strip().split()))
heapq.heapify(cao)
for i in range(m):
    heapq.heappush(cao, heapq.heappop(cao)+x)
s = heapq.heappop(cao)
sys.stdout.write(str(s))
```

### 第二题

题目描述：
我们希望一个序列中的元素是各不相同的，但是理想和现实往往是有差距的。现在给出一个序列A，其中难免有些相同的元素，现在提供了一种变化方式，使得经过若干次操作后一定可以得到一个元素各不相同的序列。
这个操作是这样的，令x为序列中最小的有重复的数字，你需要删除序列左数第一个x，并把第二个x替换为2*x。
请你输出最终的序列。
例如原序列是[2,2,1,1,1],一次变换后变为[2,2,2,1]，两次变换后变为[4,2,1]，变换结束

输入描述
输入第一行包含一个正整数n，表示序列的长度为n。(1<=n<=50000)  第二行有n个整数，初始序列中的元素。(1<=a_i<=10^8)  输出描述
输出包含若干个整数，即最终变换之后的结果。

```python
import sys, heapq
from collections import defaultdict
n = int(sys.stdin.readline().strip())
l = list(map(int, sys.stdin.readline().strip().split()))
d = defaultdict(list)
q = []
for idx, value in enumerate(l):
    d[value].append(idx)
for c in d:
    if len(d[c])>=2:
        heapq.heappush(q, c)

while q:
    t = heapq.heappop(q)
    re = heapq.heappop(d[t])
    l[re] = "a"
    n = heapq.heappop(d[t])
    l[n] *= 2
    if len(d[t]) >= 2:
        heapq.heappush(q, t)
    if t*2 in d:
        heapq.heappush(d[t*2], n)
        if len(d[t*2])>=2 and t*2 not in q:
            heapq.heappush(q, t*2)
    else:
        d[t*2] = [n]

res = " ".join(str(x) for x in l if x != "a")
sys.stdout.write(res)
```
### 第三题

堆积木
时间限制： 3000MS
题目描述：
同同在玩积木，每个积木上都有一个独一无二的二进制数字。同同的积木有特殊的规则，只有满足规则的积木才能被垒到一起。
对于一个积木，如果积木上写的数字是a，则对于所有的写着数字b的积木，只要满足a∩b=b,其中∩代表二进制与运算，则积木b可以放到积木a的上面。每一堆积木最多仅有一个积木可以充当底座，被放在底座上的所有积木都必须满足上述关系。（只要底座和积木满足关系即可以放在一堆，不需要考虑如何码放）
同同有一只笔，只能使用一次，效果是把某一个积木上数字的某一位从0改成1或者从1改成0。请问同同最少能让积木分在几堆里。
二进制与运算的定义：对于两个数a,b，写出它们的二进制形式a0a1am...和b0b1bm，设c 为与运算的结果，则有当a0和b0同时为1 时c0为1，其他位同理。

输入描述
第一行两个数n,m(1≤m≤20,1≤n≤min(105,2m))，分别代表积木的数量和积木上数字对应二进制位最多有几位
第二行n个整数a1,a2,...,an(0≤ai≤2m)，代表每个积木上写的二进制数字的十进制表示形式。
输入保证每个积木上的数字不重复。
输出描述
一行一个整数代表最少的堆数。

```python
不会
```
