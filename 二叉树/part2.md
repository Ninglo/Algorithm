## 遍历的延申问题

基于上文所讲的四种遍历方式, 可以展开许多以遍历为基础的题目, 只需套用模板并对所求数据进行记录即可.

### [二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

右视图的定义请在题库上查看, 这里介绍解题思路. 从定义很容易理解, 所谓右视图就是层次遍历中每一层的最后一个, 所以简单套用模板, 在每次遍历中添加一个检测即可.

```Python
def levelOrder(root):
    if root == None:
        return []

    res = []
    queue = [root]
    while len(queue) != 0:
        size = len(queue)
        currentLevelNode = None
        for i in range(size):
            node = queue.pop(0)
            node.left and queue.append(node.left)
            node.right and queue.append(node.right)
            currentLevelNode = node

        res.append(currentLevelNode.val)

    return res
```

### [重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

重建二叉树的关键点, 就是找到根节点. 找到根节点, 就可以把序列化结果拆成左子树部分和右子树部分, 递归处理即可.

```Python
def buildTree(preorder, inorder):
        def recur(root, left, right):
            if left > right: return
            # 递归终止
            node = TreeNode(preorder[root])
            # 建立根节点
            i = dic[preorder[root]]
            # 划分根节点、左子树、右子树
            node.left = recur(root + 1, left, i - 1)
            # 开启左子树递归
            node.right = recur(i - left + root + 1, i + 1, right)
            # 开启右子树递归
            return node                                           # 回溯返回根节点

        dic, preorder = {}, preorder
        for i in range(len(inorder)):
            dic[inorder[i]] = i
        return recur(0, 0, len(inorder) - 1)
```

### [奇偶树](https://leetcode-cn.com/problems/even-odd-tree/)

本质还是一个层次遍历的问题. 在遍历每层时做一个额外的判断即可.

```Python
    if root == None:
        return True

    queue = []
    queue.append(root)
    depth = 1
    # > 0 even  < 0 odd
    while len(queue) != 0:
        currentLevel = []
        size = len(queue)
        for i in range(size):
            let node = queue.pop(0)
            currentLevel.append(node.val)
            node.left and queue.append(node.left)
            node.right and queue.append(node.right)
        # even
        if depth > 0:
            maxVal = float("-inf")
            for i in range(len(currentLevel)):
                nowVal = currentLevel[i]
                if nowVal % 2 == 0:
                    return False
                if nowVal <= maxVal:
                    return False
                maxVal = nowVal
        # odd
        else:
            minVal = float("inf")
            for i in range(len(currentLevel)):
                nowVal = currentLevel[i]
                if nowVal % 2 == 1:
                    return False
                if nowVal >= minVal:
                    return False
                minVal = nowVal

        depth *= -1
    return True
```