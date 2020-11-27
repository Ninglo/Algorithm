# Part 1 基础模板

## 模板代码

请打开 [二分查找](https://leetcode-cn.com/problems/binary-search/)， 基于此题目理解以下代码。

```Python
import math as Math

def binarySearch(nums, target):
    # 边界条件检测
    if nums and len(nums) == 0:
        return -1

    begin = 0
    end = len(nums)
    # comment 1
    while begin + 1 < end:
        mid = Math.floor((begin + end) / 2)

        # comment 2
        if nums[mid] == target:
            return mid
        if nums[mid] > target:
            end = mid
        if nums[mid] < target:
            begin = mid

    if len(nums) > begin and nums[begin] == target:
        return begin
    if len(nums) > end and nums[end] == target:
        return end

    return -1
```

### 注解

1. 二分算法较为困惑的一点在于循环的退出条件， 为避免错误发生， 建议在 begin 指针较 end 指针小 1 的时候退出循环， 然后对 begin 和 end 指针指向的值与 target 进行两次判断。
2. 另外一个困惑的地方在于， mid 对应值与 target 比较后， begin 或 end 应如何移动。 一种降低心智负担的解就是不考虑是否 +1 或 -1 的边界优化， 在复杂度上没有变化。

### 实践

1. 阅读代码， 理解代码每一模块的含义， 自己在纸上或脑子思考代码执行过程。
2. 完成题目 [二分查找](https://leetcode-cn.com/problems/binary-search/)
3. 思考模板是如何实践二分思想的。

## 进阶

考虑 [二分查找](https://leetcode-cn.com/problems/binary-search/) 的一种特殊情况： 若 target 的数量不唯一， 若想获得 target 中第一个或最后一个的 index， 应对代码做哪些改进？

题目地址： [在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### 题解

第一个和最后一个的思想类似， 仅介绍寻找第一个的 index 思路：

实质上， 与普通二分模板相比， 该特殊情况仅仅是在判断 `nums[mid] == target` 时处理有所变化。 此时不可返回 mid， 而是 `end = mid`, 原因留作读者思考。

警示： 寻找最后一个数的代码， 最后的两个 if 判断需要反转， 请自行思考原因。

### 参考代码

```Python
import math as Math

def searchRange(nums, target):
    def findFirstTarget(nums, target):
        begin = 0
        end = len(nums)
        while begin + 1 < end:
            mid = Math.floor((begin + end) / 2)

            if nums[mid] == target:
                end = mid
            if nums[mid] > target:
                end = mid
            if nums[mid] < target:
                begin = mid

        if len(nums) > begin and nums[begin] == target:
            return begin
        if len(nums) > end and nums[end] == target:
            return end

        return -1
    
    def findEndTarget(nums, target):
        begin = 0
        end = len(nums)
        while begin + 1 < end:
            mid = Math.floor((begin + end) / 2)

            if nums[mid] == target:
                begin = mid
            if nums[mid] > target:
                end = mid
            if nums[mid] < target:
                begin = mid

        if len(nums) > end and nums[end] == target:
            return end
        if len(nums) > begin and nums[begin] == target:
            return begin

        return -1
        
    if nums and len(nums) == 0:
        return -1
    
    firstIndex = findFirstTarget(nums, target)
    endIndex = findEndTarget(nums, target)

    return [firstIndex, endIndex]
```