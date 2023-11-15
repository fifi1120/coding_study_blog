DAY1 代码随想录 。

今天学习数组。

【1】二分法的边界条件总是写错，今天第一次弄清楚了到底要怎么写：

有且仅有2种写法：

一 左闭右闭 [left,right] left和right的每次调整都是mid+1或mid-1 while left <= right

    left = 0 
    
    right = len(nums) - 1
    
    while left <= right:
    
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
    
      if target < nums[mid]:
      
        right = mid # 这里有变化
      elif target > nums[mid]:
        left = mid + 1
      else: # target == nums[mid]
        return mid
    return -1 # cannot find



【2】双指针！


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
