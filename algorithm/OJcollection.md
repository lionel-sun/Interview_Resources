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

## 科大讯飞开发岗

限制只能JAVA和C++

### 第一题 找零问题
有面值1,5,10,50,100，分别对应变量：a、b、c、d、e
若有k元钱，最少多少张纸币能找零，若不能则输出-1
贪心：从大到小遍历一遍，注意给定币值数量判断

```java
测试用例
5 2 2 3 5
55

public class Main {
 
    public static int minMoney(int[] nums,int target){
        int[] money = {1,5,10,50,100};
        int count = 0;
        for (int i=4;i>=0;i--){
            int shang = target/money[i];
            int cur_temp = Math.min(nums[i],shang);
            count += cur_temp;
            target -= cur_temp * money[i];
            if (target==0){
               return count;
            }
        }
        return -1;
    }
 
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()){
            int[] nums = new int[5];
            for (int i = 0; i < 5; i++) {
                nums[i] = sc.nextInt();
            }
            int target = sc.nextInt();
            System.out.println( minMoney(nums,target));
        }
    }
}
```

### 第二题 复现示例的排序算法

主要是能看出来是快排，快速排序，每次操作结束都要打印，每一个递归结束都要打印。

```java
测试用例
9
25 84 21 47 15 27 68 35 20
public class Main{
    static int[] data;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int num = sc.nextInt();
         data = new int[num];
        for(int i=0;i<num;i++){
            data[i]=sc.nextInt();
        }
        sort(0,num-1);
    }
 
    public static void sort(int left,int right){
        if (left>=right) return;
        int start = left;
        int end = right;
        int now = data[left];
        while (left<right){
            int n;
            int m;
            while (left<right&&data[right]>now){
                right--;
            }
            while(left<right&&data[left]<=now){
                left++;
            }
            int tmp = data[right];
            data[right] =data[left];
            data[left] =tmp;
        }
            int p = data[start];
            data[start] = data[left];
            data[left] = p;
        print();
        sort(start,left-1);
        sort(left+1,end);
    }
 
    public static void print(){
        for(int i=0;i<data.length;i++){
            if (i==data.length-1){
                System.out.println(data[i]);
                return;
            }
            System.out.print(data[i]+" ");
        }
    }
}
```

### 第三题 矩形是否相交
给两个矩形r1:(0,0)(4,3),r2:(0,1)(5,4)判断a和b是否相交。
两者不相交的情况为：r1在r2的左侧或上侧（同理，r2在r1的左侧或上侧），这里就要用到C结构体提供的界的概念了，也即r1的最右在r2最左的左侧，r1的最小在r2的最上的上侧。

```java
测试用例
0 0 4 3 0 1 5 4
1

public class Main {
 
    /**
     * 解法一：逆向思维，找不相交的情况；
     *((r1.right < r2.left) || (r1.bottom > r2.top))
	 *((r2.right < r1.left) || (r2.bottom > r1.top))
     */
    public static int isRectangleOverlap(int[] nums){
        if (nums[2]<nums[4] || nums[0] > nums[6]
                || nums[1]>nums[7] || nums[3]<nums[5]){
            return 0;
        }
        return 1;
    }
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()){
            int[] nums = new int[8];
            for (int i = 0; i < 8; i++) {
                nums[i] = sc.nextInt();
            }
            System.out.println(isRectangleOverlap(nums));
        }
    }
}

```

### 第四题 字符串中提取整数

- [leetcode 8](https://leetcode-cn.com/problems/string-to-integer-atoi/)

需要加入判断第一位是不是负号就可以了，负数保存-号


```java
测试用例：
例如：+1a2
输出：12

例如：-1a2
输出：-12
public class Main {
 
    public static void convertToInt(String s){
        int subFlag = 0;
        for (int i = 0; i < s.length(); i++) {
            if (subFlag == 0 && s.charAt(i) == '-'){
                subFlag = 1;
                System.out.print(s.charAt(i));
            }
            if (s.charAt(i)>='0' && s.charAt(i)<='9'){
                System.out.print(s.charAt(i));
            }
        }
        System.out.println(Long.parseLong(res.toString()));
    }
 
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()){
            String s = scanner.nextLine();
            convertToInt(s);
        }
    }
}
```

## 阿里

### 第一题

小强是一个农场主，农场里有n头牛，每头牛有着独一无二的体重，每一头牛的颜色可能跟是m种颜色其中的一种，小强带了一些牛（可能为0个）出来吃草。
你需要回答出小强带出来的牛的组合一共有多少种可能？

注意：因为一头牛有自己的体重（没有两头牛体重相等），所以如果四头牛的体重分别是1,2,3,4，颜色分别是y1,y2,y3,y4和
另一种方案：四头牛的体重分别是1,2,3,4颜色分别是y1,y3,y2,y4即使两个方案的颜色的种类对应的数量是相同的，但是因为颜色对应的体重不同，所以是两个不同的方案。
由于方案书可能很大，请对1e9+7取模。
输入描述：
两个整数n,m(1≤n,m≤10^9)

二分法+快速幂
```java
测试用例：
输入： 3,2

输出： 27

package org.example;
 
import java.util.Scanner;
 
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        while (in.hasNextInt()) {
            int n = in.nextInt();
            int m = in.nextInt();
            System.out.printf("%.0f\n", slice(m + 1, n));
        }
    }
 
    public static double slice(int m, int n) {
        if (n == 1) {
            return m;
        }
        double temp = slice(m, n / 2);
        return ((n % 2 == 0 ? 1 : m) * temp * temp) % 1000000007;
    }
}
```

### 第二题
小强最近在研究某个飘洋过海的游戏。

游戏可以看成一个n∗mn*mn∗m的方格图，从左上角(1,1)(1, 1)(1,1)到右下角的(n,m)(n, m)(n,m)有两种地面，CCC表示为陆地SSS表示海洋，
每次穿行只能到达上下左右四个格子之一，不能走到方格图之外。

在陆地之间穿行一格需要花费三点行动力，在海洋之间穿行一格需要花费2点行动力。
但是从陆地和从海洋到陆地穿行则需要5点行动力。

输入描述：
第一行输入三个整数n,m,表示方格图的大小和q询问次数。
随后nnn行，每行mmm个元素每个元素为'C'或'S',详见样例。

随后q行每行四个数字bx,by,ex,ey分别代表起点的坐标和终点的坐标。

这是个图算法，求最短路径。如果用DFS或者BFS可能会超时。
```java
测试用例
输入：
4 4 2
CCCS
SSSS
CSCS
SSCC
1 1 3 4
3 1 1 3

输出
13
14

DFS的解法，加了剪枝

import java.util.Scanner;
 
public class Main {
    private static boolean[][] isVisit;
    private static int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    private static int n, m, q, bx, by, ex, ey, nextX, nextY, currVal, result;
    private static String[] input;
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        m = sc.nextInt();
        q = sc.nextInt();
        input = new String[n];
        for (int i = 0; i < n; i++) {
            input[i] = sc.next();
        }
        while (q-- > 0) {
            isVisit = new boolean[n][m];
            result = Integer.MAX_VALUE;
            bx = sc.nextInt();
            by = sc.nextInt();
            ex = sc.nextInt();
            ey = sc.nextInt();
            currVal = 0;
            BackTrace(bx, by);
            System.out.println(result);
        }
    }
 
    public static boolean isOk(int x, int y) {
        return x >= 0 && x < n && y >= 0 && y < m && !isVisit[x][y];
    }
 
    public static void BackTrace(int x, int y) {
        if (currVal >= result) {
            return;
			# 剪枝
        }
        if (x == ex && y == ey) {
            if (currVal < result) {
                result = currVal;
            }
            return;
        }
        for (int i = 0; i < 4; i++) {
            nextX = x + direction[i][0];
            nextY = y + direction[i][1];
            if (isOk(nextX, nextY)) {
                int add = 2;
                if (input[x].charAt(y) != input[nextX].charAt(nextY)) {
                    add = 5;
                } else if (input[nextX].charAt(nextY) == 'C') {
                    add = 3;
                }
                isVisit[x][y] = true;
                currVal += add;
                BackTrace(nextX, nextY);
                currVal -= add;
                isVisit[x][y] = false;
            }
        }
    }
}

```

## 拼多多

### 第一题

飞行旗，暴力解法列出所有情况就行了。

```python
import sys

k, n = list(map(int, sys.stdin.readline().strip().split()))
l = list(map(int, sys.stdin.readline().strip().split()))
j = 0
if k == 0: print("paradox")
else:
    for i in range(n):
        tmp = k - l[i]
        if tmp < 0:
            k = 0-tmp
            j+=1
            continue
        if tmp == 0:
            k=0
            if i == n-1:
                print(str(k)+" "+str(j))
            elif i < n-1:
                print("paradox")
            break
        k = tmp
    if k != 0 :
        print(str(k)+" "+str(j))
```

### 第二题

两个骰子如果同类，则通过上下，左右，前后反转，它们都相同，求不同种类的骰子的数量各是多少？

暴力破解。枚举，找个1在固定位置，然后比对周围的数字

```python
输入：
2
1 2 3 4 5 6
1 2 6 5 3 4
输出
1
2
import sys
from collections import defaultdict
# 上下左右前后对应索引位置：0,1,2,3,4,5
counter = defaultdict(int)
n = int(sys.stdin.readline().strip())
for i in range(n):
    nums = list(map(int, sys.stdin.readline().strip().split()))
    for idx, val in enumerate(nums):
        if val == 1:
            if idx == 0:
                break
            elif idx == 1:
                nums[0], nums[1], nums[4], nums[5] = nums[1], nums[0], nums[5], nums[4]
            elif idx == 2:
                nums[0], nums[1], nums[2], nums[3] = nums[2], nums[3], nums[1], nums[0]
            elif idx == 3:
                nums[0], nums[1], nums[2], nums[3] = nums[3], nums[2], nums[0], nums[1]
            elif idx == 4:
                nums[0], nums[1], nums[4], nums[5] = nums[4], nums[5], nums[1], nums[0]
            elif idx == 5:
                nums[0], nums[1], nums[4], nums[5] = nums[5], nums[4], nums[0], nums[1]
            break
    numf2 = nums[0]+1
    if nums[1] == numf2:
        numf2+=1
    for idx, val in enumerate(nums):
        if val == numf2:
            if idx == 4:
                break
            elif idx == 2:
                nums[2], nums[3], nums[4], nums[5] = nums[5], nums[4], nums[2], nums[3]
            elif idx == 3:
                nums[2], nums[3], nums[4], nums[5] = nums[4], nums[5], nums[3], nums[2]
            elif idx == 5:
                nums[2], nums[3], nums[4], nums[5] = nums[3], nums[2], nums[5], nums[4]
    counter[tuple(nums)] +=1
    print(nums)

res = [x for i, x in counter.items()]
res.sort(reverse=True)
print(len(res))
print(" ".join(str(x) for x in res))
```

### 第三题
吃东西，N,M是中餐和晚餐的个数，T是需要满足的最少的美味值
输入：N行，每行两个数，xi和yi分别表示热量和美味值，再输入M行，每行两个数，xi和yi分别表示热量和美味值。
输出：中餐和晚餐的美味值>=T的情况下，热量值是多少？

暴力枚举只能过50%后面超时。先按照热量，将早餐和晚餐进行排序。 然后单独判断午餐，找到符合要求的美味值后，后面剩余的都不需要遍历了，单独判断晚餐的情况同理。
对于午餐和晚餐都选的情况，当找出复合要求的晚餐后，对于后续的遍历，就不需要遍历该晚餐之后的晚餐了。（这种方法可以AC）

```python
测试用例：
输入:  
5 1 9
9 1
4 9
3 1
2 3
6 5
9 8 
输出:4 （解释：只需要吃一顿中餐即可） 

示例 2:
输入: 
1 1 0
3 1
2 1
输出: 0（解释，一顿都不吃）

示例 3:
输入:
3 3 10
1 1
2 5
3 7
2 4
4 8
6 9
输出: 5

示例 4:
输入: 
2 1 4
3 1
2 1
1 2
输出:
-1(解释怎么都达不到美味值，输出-1) 

import sys

n, m, k = list(map(int, sys.stdin.readline().strip().split()))
Alist, Blist = [],[]
res = float('inf')
for i in range(n):
    Alist.append(list(map(int, sys.stdin.readline().strip().split())))
for i in range(m):
    Blist.append(list(map(int, sys.stdin.readline().strip().split())))
Alist.sort(key=lambda x:x[0])
Blist.sort(key=lambda x:x[0])

if not k: print(0)
else:
    for i in Alist:
        if i[1] >= k:
            res = min(res, i[0])
            break
    for i in Blist:
        if i[1] >= k:
            res = min(res, i[0])
            break
    if res != float('inf'): print(res)
    else:
        for i in Alist:
            for j in Blist:
                if i[1]+j[1]>=k:
                    res = min(res, i[0]+j[0])
                    break
        if res == float('inf'): print(-1)
        else: print(res)
```

### 第四题
6*6的图输入，#表示这块地没有建筑，*表示这块地有建筑，没建筑的地方种6种植物，前后左右不能一样，求有多少种种植方案。

输出对10000000009取余

没思路，还没看到大佬的解题呢

```python
```
