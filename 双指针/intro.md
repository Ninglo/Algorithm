# 双指针

## 1. [调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/solution/)

算法流程：

    初始化： i , j 双指针，分别指向数组 nums 左右两端；
    循环交换： 当 i=j 时跳出；
        指针 i 遇到奇数则执行 i=i+1 跳过，直到找到偶数；
        指针 j 遇到偶数则执行 j=j−1 跳过，直到找到奇数；
        交换 nums[i]n 和 nums[j] 值；
    返回值： 返回已修改的 nums 数组。

```Python
class Solution:
    def exchange(self, nums: List[int]) -> List[int]:
        i, j = 0, len(nums) - 1
        while i < j:
            while i < j and nums[i] & 1 == 1: i += 1
            while i < j and nums[j] & 1 == 0: j -= 1
            nums[i], nums[j] = nums[j], nums[i]
        return nums
```

## 2. [二维数组查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

算法流程：

    从矩阵 matrix 左下角元素（索引设为 (i, j) ）开始遍历，并与目标值对比：
        当 matrix[i][j] > target 时，执行 i-- ，即消去第 i 行元素；
        当 matrix[i][j] < target 时，执行 j++ ，即消去第 j 列元素；
        当 matrix[i][j] = target 时，返回 true ，代表找到目标值。
    若行索引或列索引越界，则代表矩阵中无目标值，返回 false 。

    每轮 i 或 j 移动后，相当于生成了“消去一行（列）的新矩阵”， 索引(i,j) 指向新矩阵的左下角元素（标志数），因此可重复使用以上性质消去行（列）。

```Python
class Solution:
    def findNumberIn2DArray(self, matrix: List[List[int]], target: int) -> bool:
        i, j = len(matrix) - 1, 0
        while i >= 0 and j < len(matrix[0]):
            if matrix[i][j] > target: i -= 1
            elif matrix[i][j] < target: j += 1
            else: return True
        return False
```

## 3. [移动零](https://leetcode-cn.com/problems/move-zeroes/)

使用双指针，左指针指向当前已经处理好的序列的尾部，右指针指向待处理序列的头部。

右指针不断向右移动，每次右指针指向非零数，则将左右指针对应的数交换，同时左指针右移。

注意到以下性质：

    左指针左边均为非零数；

    右指针左边直到左指针处均为零。

因此每次交换，都是将左指针的零与右指针的非零数交换，且非零数的相对顺序并未改变。

```Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        n = len(nums)
        left = right = 0
        while right < n:
            if nums[right] != 0:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
            right += 1
```

## 4. [旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

算法流程：

    初始化： 声明 i, j 双指针分别指向 nums 数组左右两端；
    循环二分： 设 m=(i+j)/2 为每次二分的中点（ "/" 代表向下取整除法，因此恒有 i≤m<j ），可分为以下三种情况：
        当 nums[m]>nums[j] 时： m 一定在 左排序数组 中，即旋转点 x 一定在 [m+1,j] 闭区间内，因此执行 i=m+1；
        当 nums[m]<nums[j] 时： m 一定在 右排序数组 中，即旋转点 x 一定在[i,m] 闭区间内，因此执行 j=m；
        当 nums[m]=nums[j] 时： 无法判断 m 在哪个排序数组中，即无法判断旋转点 x 在 [i,m] 还是 [m+1,j] 区间中。解决方案： 执行 j = j - 1 缩小判断范围，分析见下文。
    返回值： 当 i=j 时跳出二分循环，并返回 旋转点的值 nums[i] 即可。

```Python
class Solution:
    def minArray(self, numbers: [int]) -> int:
        i, j = 0, len(numbers) - 1
        while i < j:
            m = (i + j) // 2
            if numbers[m] > numbers[j]: i = m + 1
            elif numbers[m] < numbers[j]: j = m
            else: j -= 1
        return numbers[i]
```