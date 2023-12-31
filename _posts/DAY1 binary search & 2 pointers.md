DAY1 代码随想录训练营 数组1/2

# 二分搜索

【1】二分法的边界条件总是写错，今天第一次弄清楚了到底要怎么写：

有且仅有2种写法：

### 左闭右闭 [left,right] left和right的每次调整都是mid+1或mid-1 while left <= right --> 我选定这一种

【顺口溜：左闭右闭，加一减一，小于等于。】

    left = 0 
    
    right = len(nums) - 1
    
    while left <= right:

      mid = left + (right - left) // 2  # mid要在while循环里面，这样才能不断更新
    
      if target < nums[mid]:
      
        right = mid - 1
        
      elif target > nums[mid]:
      
        left = mid + 1
        
      else: # target == nums[mid]
      
        return mid
        
    return -1 # cannot find


### 左闭右开 [left,right) left和right的调整为mid+1和mid while left < right 

    left = 0 
    
    right = len(nums) # 这里有变化
    
    while left < right: # 这里有变化

      mid = left + (right - left) // 2  # mid要在while循环里面，这样才能不断更新
    
      if target < nums[mid]:
      
        right = mid # 这里有变化
      elif target > nums[mid]:
        left = mid + 1
      else: # target == nums[mid]
        return mid
    return -1 # cannot find


## 经典题704 一定要写得肌肉记忆

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l = 0 
        r = len(nums)-1

        while l <= r:
            mid = l + (r-l)//2
            if target < nums[mid]:
                r = mid - 1
            elif target > nums[mid]:
                l = mid + 1
            else:
                return mid
        return -1
```

## 进阶题（很棒哦自己写出来了）74 在matrix里面二分法，其实就是先对行二分法，再对列二分法。

```
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        row = len(matrix) #row=1
        col = len(matrix[0]) #col=1
        top = 0
        bot = row - 1 # 2 #bot = 0
        while top <= bot:
            mid_row = top + (bot - top) // 2 #mid_col=0+1=1
            if target > matrix[mid_row][-1]:
                top = mid_row + 1
            elif target < matrix[mid_row][0]:
                bot = mid_row - 1
            else: #target在mid那一行了
                left = 0 
                right = len(matrix[0])
                while left <= right:
                    mid_col = left + (right - left)//2
                    if target < matrix[mid_row][mid_col]:
                        right = mid_col - 1
                    elif target > matrix[mid_row][mid_col]:
                        left = mid_col + 1
                    else:
                        return True
                return False
```

## 自己不会做的easy题 441. 排列硬币

求的是 sum(k) = k * (k + 1) / 2 <= n中最大的k，且存在二段性：对于某个 k1，sum(k1) <= n，而sum(k2) > n，k2 > k1。可以用二分的查找最大满足条件的模板。

```
class Solution:
    def arrangeCoins(self, n: int) -> int:
        #1,3,6,10,15，。。。。
        #其实每一行是：1,2,3,4,5是等差数列。等差数列前n项和：（首项+末项）*项数/2=（1+n)n/2
        #其实这题的二分法比较隐晦，但是其实仔细思考题目的特点有：有序单调（有规律）、有最直白的界限（反正答案不会超过n）、题目要你找满足XXX条件的边界---> 二分法！
        #难点是：等差数列求和公式+发现二分法
        left = 0
        right = n
        while left <= right:
            mid = left + (right - left) // 2
            if (1+mid) * mid // 2 > n:
                right = mid - 1
            elif (1+mid) * mid // 2 < n:
                left = mid + 1
            else:
                return mid
        return left-1
```

# 快慢指针


其实一般是双指针和while loop搭配，根据情况看要不要＋1，就不用搞for loop混淆了。

快慢指针可以用在删除数组元素的题目里，快的指针做一个while loop，把整个数组loop一遍，每次都正常+1；

慢的指针就是如果不是要删除的值，正常+1，并且更新nums[slow]的值；如果正好是要删除的值，slow就不往前走也不更新。


## 27 移除元素

这一题需要原地修改一个list（删除），同时还要返回剩余的长度。记住，原地删除有一个经典方法是：通过index去修改。如果还要返回剩余长度，联想到指针+index。


```
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int: #原地删除所有值为val的数
        slow = 0
        fast = 0
        while fast <= len(nums)-1:
            if nums[fast] == val: # need to be deleted （其实换成指针语言就是：如果这个值是要被删除的，那就fast继续往前走，直到找到下一个不需要被删除的元素，然后存在slow那里
                fast += 1
            else: # 这就是正常找到了一个要存进slow里的
                nums[slow] = nums[fast]
                slow += 1
                fast += 1
        return slow
```

