## 链表：简单题

### [从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

首先求得链表长度， 声明等于长度的数组， 顺序遍历并倒序插入数组即可。

```Python
def getLinkLength(head):
    length = 0
    while head:
        head = head.next
        length += 1
    return length

def reversePrint(head):
    length = getLinkLength(head)
    res = [None] * length
    index = length-1

    while head:
        res[index] = head.val
        index -= 1
        head = head.next

    return res
```

### [链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/submissions/)

同样的道理， 先求得链表长度， 链表查找`length-k`次， 即为需返回的头节点。

```Python
def getKthFromEnd(head, k):
        def getLinkLength(head):
            length = 0
            while head:
                head = head.next
                length += 1
            return length
        
        length = getLinkLength(head)
        for i in range(length-k):
            head = head.next

        return head
```

### 环形链表

表面看是简单题， 但是实话说空间为`O(1)`的解法确实非常人所能想出。 快慢指针在环形链表一定会相遇， 原理不予证明， 理念可以参考代码理解。

```Python
def hasCycle(head):
    fast = head
    slow = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if fast == slow:
            return True

    return False
```