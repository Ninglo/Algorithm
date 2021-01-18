## 1. [数字组合](https://www.lintcode.com/problem/combination-sum/description)

解题思路

    本题需要在给定一个候选数字的集合 candidates 中. 找到所有的和为目标值 target 的组合。
    对于这种需要求解所有结果的问题，我们为了避免结果有遗漏或者重复，一般可以考虑使用搜索算法。而本题需要满足组合中选取的元素之和为一个目标值，这个可以作为深度优先搜索的边界判断条件，故本题可以使用深度优先搜索（DFS）算法。

算法流程

    由于题目候选数字的集合candidates可能包含重复数字，且返回的结果要求组合内数字非降序，因此首先需对candidates进行升序排序并去重，得到新的数字集合candidatesNew；
    对新的数字集合进行深度优先搜索，传入的参数包括：数字集合candidatesNew、当前的位置index、当前存入的组合current、距离目标值的差remainTarget、保存答案的列表result；
        当remainTarget=0即达到边界，将current添加到result，回溯；
        循环遍历index位置到数字集合的末尾，分别递归调用dfs；
        递归步进为：remainTarget - candidatesNew[i]；
        剪枝：当发现当前的数字加入已超过remainTarget可进行剪枝。

```Python
class Solution:
    # @param candidates, a list of integers
    # @param target, integer
    # @return a list of lists of integers
    def combinationSum(self, candidates, target):
        results = []

        # 集合为空
        if len(candidates) == 0:
            return results

        # 利用set去重后排序
        candidatesNew = sorted(list(set(candidates)))

        # dfs
        self.dfs(candidatesNew, 0, [], target, results)

        return results

    def dfs(self, candidatesNew, index, current, remainTarget,  results):
        # 到达边界
        if remainTarget == 0:
            return results.append(list(current))

        # 递归的拆解：挑一个数放入current
        for i in range(index, len(candidatesNew)):

            # 剪枝
            if remainTarget < candidatesNew[i]:
                break

            current.append(candidatesNew[i])
            self.dfs(candidatesNew, i, current, remainTarget - candidatesNew[i], results)
            current.pop()
```

## 2. [数字组合II](https://www.lintcode.com/problem/combination-sum-ii/description)

```Python
class Solution:
    """
    @param candidates: Given the candidate numbers
    @param target: Given the target number
    @return: All the combinations that sum to target
    """
    def combinationSum2(self, candidates, target):
        # write your code here
        candidates.sort()
        self.ans, tmp, use = [], [], [0] * len(candidates)
        self.dfs(candidates, target, 0, 0, tmp, use)
        return self.ans
    def dfs(self, can, target, p, now, tmp, use):
        if now == target:
            self.ans.append(tmp[:])
            return
        for i in range(p, len(can)):
            if now + can[i] <= target and (i == 0 or can[i] != can[i-1] or use[i-1] == 1):
                tmp.append(can[i])
                use[i] = 1
                self.dfs(can, target, i+1, now + can[i], tmp, use)
                tmp.pop()
                use[i] = 0
```
