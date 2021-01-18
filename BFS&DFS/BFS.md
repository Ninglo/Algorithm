## 1. [二叉树的序列化与反序列化](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

序列化(层序遍历)的本质就是 BFS, 这部分内容请看二叉树章节, 简述一下反序列化.

算法流程：

    特例处理： 若 data 为空，直接返回 null ；
    初始化： 序列化列表 vals （先去掉首尾中括号，再用逗号隔开），指针 i = 1 ，根节点 root （值为 vals[0] ），队列 queue（包含 root ）；
    按层构建： 当 queue 为空时跳出；
        节点出队，记为 node ；
        构建 node 的左子节点：node.left 的值为 vals[i] ，并将 node.left 入队；
        执行 i += 1 ；
        构建 node 的右子节点：node.left 的值为 vals[i] ，并将 node.left 入队；
        执行 i += 1 ；
    返回值： 返回根节点 root 即可；

```Python
class Codec:
    def serialize(self, root):
        if not root: return "[]"
        queue = collections.deque()
        queue.append(root)
        res = []
        while queue:
            node = queue.popleft()
            if node:
                res.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
            else: res.append("null")
        return '[' + ','.join(res) + ']'

    def deserialize(self, data):
        if data == "[]": return
        vals, i = data[1:-1].split(','), 1
        root = TreeNode(int(vals[0]))
        queue = collections.deque()
        queue.append(root)
        while queue:
            node = queue.popleft()
            if vals[i] != "null":
                node.left = TreeNode(int(vals[i]))
                queue.append(node.left)
            i += 1
            if vals[i] != "null":
                node.right = TreeNode(int(vals[i]))
                queue.append(node.right)
            i += 1
        return root
```

二叉树的 BFS 实质就是层序遍历, 只要掌握层序遍历就没什么复杂的, 在此给出三道练习题供参考:

1. [二叉树的层次遍历](https://www.lintcode.com/problem/binary-tree-level-order-traversal-ii/description)
2. [二叉树的锯齿形层次遍历](https://www.lintcode.com/problem/binary-tree-zigzag-level-order-traversal/description)
3. [将二叉树按照层级转化为链表](https://www.lintcode.com/problem/convert-binary-tree-to-linked-lists-by-depth/description)

## 5. [拓扑排序](https://www.lintcode.com/problem/topological-sorting/description)

算法思路

    在图中从顶点A到顶点B有一条有向路径，则顶点A一定排在顶点B之前，满足这样的条件的顶点序列称为一个拓扑序。
    拓扑排序有两个步骤：
    1.从队列中获取一个入度为0的顶点
    2.获取该顶点边，将边的另一端顶点入度减一，如果为0，也入队列
    重复步骤1和2，直到队列为空，得到的出队顺序即为一个合理的拓扑序。

```Python
class Solution:
    """
    @param: graph: A list of Directed graph node
    @return: Any topological order for the given graph.
    """
    def topSort(self, graph):
        topu = []
        # in记录节点的入度
        in_degree = {}
        que = collections.deque()
        for e in graph:
            for i in e.neighbors:
                # 记录每个点的入度
                if i in in_degree:
                    in_degree[i] += 1
                else:
                    in_degree[i] = 1
        for e in graph:
            #  入度为0的点作为起始点
            if e not in in_degree:
                que.append(e)
        while len(que) > 0:
            now = que.popleft()
            topu.append(now)
            for e in now.neighbors:
                in_degree[e] -= 1
                if in_degree[e] == 0:
                    que.append(e)
        return topu
```

## 6. [岛屿个数](https://www.lintcode.com/problem/number-of-islands/description)

定义一个矩阵记录地图状态, 还需要定义初始含 n×m

个集合的并查集, 以及一个计数器, 记录当前岛屿的个数.

对于每一次操作(x, y), 如果(x, y)的上下左右都是0, 那么计数器加一; 如果不全为0, 则:

    并查集查询其四周的1所属的集合, 假设它们属于 k 个不同的集合
    计数器减去 k-1
    将这 k 个集合, 连同 (x, y), 合并

并查集的实现可以利用Point结构体, 也可以将二维坐标转换成一维.

```Python
DIRECTIONS = [(0, -1), (0, 1), (-1, 0), (1, 0)]

class Solution:
    """
    @param n: An integer
    @param m: An integer
    @param operators: an array of point
    @return: an integer array
    """
    def numIslands2(self, n, m, operators):
        results = []
        island = set()
        self.size = 0
        self.father = {}
        for operator in operators:
            x, y = operator.x, operator.y
            if (x, y) in island:
                results.append(self.size)
                continue

            island.add((x, y))
            self.father[(x, y)] = (x, y)
            self.size += 1
            for delta_x, delta_y in DIRECTIONS:
                x_ = x + delta_x
                y_ = y + delta_y
                if (x_, y_) in island:
                    self.union((x_, y_), (x, y))

            results.append(self.size)

        return results

    def union(self, point_a, point_b):
        root_a = self.find(point_a)
        root_b = self.find(point_b)
        if root_a != root_b:
            self.father[root_a] = root_b
            self.size -= 1

    def find(self, point):
        path = []
        while point != self.father[point]:
            path.append(point)
            point = self.father[point]

        for p in path:
            self.father[p] = point

        return point
```
