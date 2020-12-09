## DummyNode 型题目

### 介绍

所谓`DummyNode`， 就是在链表头节点前再新建一个节点， 保证对链表操作完成之后， 还能获取链表头部的指针。一般需要对链表的修改的题目都可用此方法。

### 例题

#### [删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

以此题为例， 首先 new 一个 DummyNode， 定义两个链表指针`prev` and `curt`， 开始遍历链表。 当扫描到等于给定`val`的节点时， 将该节点的前一个节点连接至此节点的下一个节点， 最后返回头节点。

修改效果如下：
```
if curt.val == val:

prev  curt(need del)   curt.val
  |                        ^
  -------------------------|
```


```Python
def deleteNode(head, val):
    dummy = ListNode(0)
    dummy.next = head

    prev = dummy
    curt = head
    while curt:
        if curt.val == val:
            prev.next = curt.next
        prev = prev.next
        curt = curt.next

    return dummy.next
```

### [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

先创建一个新的`DummyNode`作为合并后链表的头结点。 然后对 l1 l2 依次比较， 将较小值添入新链表， 更新有较小值的链表头部， 如此往复即可。

```Python
def mergeTwoLists(l1, l2):
    dummy = ListNode(0)
    curt = dummy
    while l1 and l2:
        if l1.val < l2.val:
            curt.next = l1
            l1 = l1.next
            curt = curt.next
        else:
            curt.next = l2
            l2 = l2.next
            curt = curt.next
    
    if l1:
        curt.next = l1
    if l2:
        curt.next = l2
    
    return dummy.next
```