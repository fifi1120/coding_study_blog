DAY1 代码随想录训练营 数组1/2

# 二分搜索

【1】二分法的边界条件总是写错，今天第一次弄清楚了到底要怎么写：

有且仅有2种写法：

一 左闭右闭 [left,right] left和right的每次调整都是mid+1或mid-1 while left <= right --> 我选定这一种

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


二 左闭右开 [left,right) left和right的调整为mid+1和mid while left < right 

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
···


# 双指针（快慢指针）


其实一般是双指针和while loop搭配，根据情况看要不要＋1，就不用搞for loop混淆了。

快慢指针可以用在删除数组元素的题目里，快的指针做一个while loop，把整个数组loop一遍，每次都正常+1；

慢的指针就是如果不是要删除的值，正常+1，并且更新nums[slow]的值；如果正好是要删除的值，slow就不往前走也不更新。


#27
```
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        # when using two pointers, no need to use for loop -> confusing. just let two pointers + 1 or not.
        slow = 0
        fast = 0
        while fast <= len(nums) - 1: #while fast < 7
            if nums[fast] != val: 
                nums[slow] = nums[fast]
                fast += 1 #fast = 1,2
                slow += 1 #slow = 1,2
            else:
                fast += 1 #fast=3
        return slow
```

