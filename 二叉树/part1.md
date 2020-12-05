## 二叉树的遍历

二叉树的遍历，就是把树形的存储结构(二叉树的结构)， 转换为线性的结构(即数组)。 常见的遍历手段有四种：

1. [前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
2. [中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
3. [后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)
4. [层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

### X序遍历

前三种遍历手段非常相似。其中， 前序遍历首先访问根结点然后遍历左子树，最后遍历右子树。 中序遍历先左再根节点再右， 后序遍历为左子树->右子树->根节点。 这里仅给出前序遍历的递归 & 迭代代码， 剩余代码请读者照例自行写出。

```Python
# recursion
def preorderTraversal(root):
    if root == None:
        return []

    left = preorderTraversal(root.left)
    right = preorderTraversal(root.right)
    return [root.val] + left + right

# iteration
def preorderTraversal(root):
    if root == None:
        return []

    res = []
    stack = [root]
    while len(stack) != 0:
        node = stack.pop()
        res.append(node.val)
        node.right and stack.append(node.right)
        node.left and stack.append(node.left)

    return res
```

简述一下算法的思路：

1. 递归算法
   思路很简单， `preorderTraversal` 函数返回一个节点的 X序遍历， 那么结果实质就是把左右叶子的遍历结果和根节点的值拼起来返回， 设置好截止条件即可。
2. 迭代算法
   以前序遍历为例。在将根值压入`res`数组之后， 只要保证所有右节点都晚于左节点访问， 就可以保证返回数组的顺序是 根值->左子叶的前序遍历结果->右子叶的前序遍历结果， 即给出的模板。

### 层次遍历

层序遍历是一种较为特殊的遍历手段， 其遍历方式为从根节点， 层层遍历。此遍历手段迭代写法较为简单， 因而便仅给迭代写法的模板：

```Python
def levelOrder(root):
    if root == None:
        return []

    res = []
    queue = [root]
    while len(queue) != 0:
        currentLevel = []
        # comment 1
        size = len(queue)
        for i in range(size):
            node = queue.pop(0)
            currentLevel.append(node.val)
            node.left and queue.append(node.left)
            node.right and queue.append(node.right)
        
        res.append(currentLevel)

    return res
```

容易看出， 层次遍历迭代写法和 X序遍历的迭代写法较为相似， 值得一提的是`comment1`， size 的含义是获取下一层节点的数量， 保证新 push 进入队列的节点不会进入 `currentLevel`。