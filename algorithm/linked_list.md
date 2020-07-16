# 链表

## [删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head: return None
        node = head
        while node.next:
            if node.next.val == node.val:
                node.next = node.next.next
            else:
                node = node.next
        return head
```

## [删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
> 注意要删除头结点，用 dummy node 辅助删除。pre放在前一个确定保留的节点，cur无限循环只要一样就跳过去。一直找到不一样的点。
```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head: return None
        dummy = ListNode(next=head)
        pre, cur = dummy, head

        while cur and cur.next:
            if cur.val == cur.next.val:
                while cur.next and cur.val == cur.next.val:
                    cur = cur.next
                pre.next, cur = cur.next, cur.next
            else:
                pre, cur = pre.next, cur.next

        return dummy.next
```

## [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
迭代方法
```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head: return None
        pre = None
        cur = head
        while cur:
            tmp = cur.next
            cur.next = pre
            pre, cur = cur, tmp
        return pre
```
递归方法
```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        
        cur = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return cur
```

## [反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
迭代
```python
class Solution:
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        if not head: return None

        dummy = ListNode(next=head)
        pre = dummy
        for i in range(m-1):
            pre = pre.next
        node = None
        cur = pre.next
        for i in range(n-m+1):
            tmp = cur.next
            cur.next = node
            node = cur
            cur = tmp
        pre.next.next = cur
        pre.next = node
        return dummy.next
```

## [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode()
        cur = dummy
        while l1 and l2:
            if l1.val > l2.val:
                cur.next, l2 = l2, l2.next
            else:
                cur.next, l1 = l1, l1.next
            cur = cur.next
        if l1:
            cur.next = l1
        if l2:
            cur.next = l2

        return dummy.next
```

## [分隔链表](https://leetcode-cn.com/problems/partition-list/)

```python
class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        if not head: return None
        l = l_head = ListNode()
        r = r_head = ListNode()
        while head:
            if head.val<x:
                l.next = head
                l = l.next
            else:
                r.next = head
                r = r.next
            head = head.next
        l.next = r_head.next
        r.next = None
        return l_head.next
```
> 当头结点不确定时候使用dummy哑巴节点

## [排序链表](https://leetcode-cn.com/problems/sort-list/)
> 使用递归排序，时间复杂度O(nlogn) 
```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next: return head
        slow, fast = head, head.next
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
        mid, slow.next =  slow.next, None
        left,right = self.sortList(head), self.sortList(mid)
        h = res = ListNode()
        while left and right:
            if left.val > right.val:
                h.next, right = right, right.next
            else:
                h.next, left = left, left.next
            h = h.next
        h.next = right if right else left
        return res.next
```

## [重排链表](https://leetcode-cn.com/problems/reorder-list/)
> 先找中点，后半段反转，然后插入前半段。空间复杂度O(1)
```python
class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head: return None
        slow, fast = head, head.next
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
        mid, slow.next = slow.next, None
        
        pre, cur = None, mid
        while cur:
            tmp = cur.next
            cur.next = pre
            pre, cur = cur, tmp
        
        while head and pre:
            tmp = head.next
            head.next = pre
            pre = pre.next
            head = head.next
            head.next = tmp
            head = head.next

        return 
```

## [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
使用额外空间O(n)
```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        s = set()
        while head:
            if head in s:
                return True
            s.add(head)
            head = head.next
        return False
```
不使用额外空间
```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
            if fast is slow:
                return True
        return False 
```


## [环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
使用额外空间O(n)，和上一题一样
```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        s = set()
        while head:
            if head in s:
                return head
            s.add(head)
            head = head.next
        return None
```
不使用额外空间，当fast和slow相遇，slow回归头结点以相同速度前进，再相遇就是入环节点。
```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        slow, fast = head, head
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
            if slow is fast:
                slow = head
                while slow is not fast:
                    slow, fast = slow.next, fast.next
                return slow
        return None
```
> fast 如果初始化为 head.Next 则中点在 slow.Next, fast 初始化为 head,则中点在 slow

## [回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)
使用额外空间O(n),不改变原链表。
```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        s = []
        slow = fast = head
        while fast and fast.next:
            s.append(slow.val)
            slow, fast = slow.next, fast.next.next
        if fast:
            slow = slow.next
        while len(s)>0:
            if s.pop() != slow.val:
                return False
            slow = slow.next
        return True
```

## [复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
使用哈希表key是旧的点，值是新的点。python中用字典
```python
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head: return None
        d, old = {}, head
        while old:
            d[old] = Node(old.val)
            old = old.next
        old = head
        while old:
            if old.next:
                d[old].next = d[old.next]
            if old.random:
                d[old].random = d[old.random]
            old = old.next
        return d[head]
```
将新节点加到旧的后面，然后复制随机指针，后删除旧节点
```python
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head: return None
        old = head
        while old:
            tmp = Node(old.val)
            tmp.next = old.next
            old.next = tmp
            old = tmp.next
        
        old = head
        while old:
            tmp = old.next.next
            old.next.next = tmp.next if tmp else None
            old.next.random = old.random.next if old.random else None
            old = tmp
        return head.next
```


## [相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
> 增加难度版本需要判断是否有环，本题默认无环

使用双指针链表拼接，当a链表指针结束指向b开头，当b链表指针结束指向a开头。两个指针节点相同就是交点（没交点就都是None的时候返回）
```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        ha, hb = headA, headB
        while ha != hb:
            ha = ha.next if ha else headB
            hb = hb.next if hb else headA
        return ha
```
使用哈希表(python中set集合)，b中节点如果在a中节点的集合中就返回。没有就返回空值。
```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        ha, hb = headA, headB
        s = set()
        while ha:
            s.add(ha)
            ha = ha.next
        while hb:
            if hb in s:return hb
            hb = hb.next
        return None
```