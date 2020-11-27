# Part 2 拓展

## x 的平方根

### 题目

[x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

### 思路

易知， 一个正数的平方根整数部分必然在 [1, floor(x / 2) + 1] 区间内， 而且有且仅有一个解， 符合二分查找的要求， 则代码易得。

```Python
def mySqrt(self, x: int) -> int:
    if x == 0:
        return x

    begin = 1
    end = Math.floor(x / 2) + 1
    while (begin + 1 < end):
        mid = Math.floor((begin + end) / 2)

        if mid * mid == x:
            return mid
        elif mid * mid > x: 
            end = mid
        else:
            begin = mid
        
    return begin
```

## 猜数字大小

### 题目

[猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

### 思路

逻辑与导入中智力题思路基本一致， 依旧是套用模板即可。

```Python
import math as Math

def guessNumber(n):
    begin = 1
    end = n
    while begin + 1 < end:
        mid = Math.floor((begin + end) / 2)
        curt = guess(mid)

        if curt == 0:
            return mid
        if curt == 1:
            begin = mid
        if curt == -1:
            end = mid

    return begin if guess(begin) == 0 else end
```