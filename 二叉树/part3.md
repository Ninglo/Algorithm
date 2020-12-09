## 进阶习题

### [LCA](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

挺有趣的一道题目, 简洁的解法很难自己想出来, 可结合代码理解. 首先观察代码, 可知若节点与左右子树中不存在 p or q, 则返回`None`, 那么若一个节点(记为`node1`)为 p or q, 且左右子树中不含 p or q, 则函数必返回自身.  若`node1`的父节点的另外一颗子树无 p or q, 则父节点的函数也会返回`node1`. 如此, 便实现了对 p or q 的传递, 若某个节点左右子树分别传递了 p and q, 那么这个节点就是`LCA`. 因为 p and q 有且仅有一个, 那么之后的传递也一直会传递这个`LCA`节点.

```Python
def lowestCommonAncestor(root, p, q):
    if root == None or root == p or root == q:
        return root

    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)

    if left and right:
        return root

    return left if left else right
```

### [反转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

非常简单的一道习题. 对左右子树位置反转, 并且对左右子树递归进行反转过程即可.

```Python
def invertTree(root):
        if not root:
            return None

        def reverse(root):
            if not root:
                return
            root.left, root.right = root.right, root.left
            reverse(root.left)
            reverse(root.right)

        reverse(root)

        return root
```