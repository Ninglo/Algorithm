## 简单的辅助函数

由于链表类题目大多是模拟法, 所以代码较其他部分会复杂一些, 为减轻心智负担, 在此给出一些辅助函数使流程更加清晰.

### [反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

```Python
def reverseList(head):
    curt = head
    prev = None
    while curt:
        temp = curt.next
        curt.next = prev
        prev = curt
        curt = temp
    
    return prev
```

### 获得链表长度

```Python
def getLinkLen(head):
    length = 0
    while head:
        head = head.next
        length += 1
    
    return length
```
