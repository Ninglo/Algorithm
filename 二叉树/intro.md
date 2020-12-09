# 二叉树 Binary Tree

## 所需前置知识

1. 对`指针`有基础的认识
2. 了解递归
3. 熟悉`struct` or `class`
4. 对树结构有简单了解

## 二叉树介绍

二叉树自身的定义， 实际上就是一个递归的结构， 一个简单的定义代码如下：
```Python
class Tree:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

## 课程结构

1. [二叉树的遍历](part1.md)
2. [遍历的延申问题](part2.md)
3. [拓展问题](part3.md)