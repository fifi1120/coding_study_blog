##  454.四数相加II 

这道题就是两两（n^2) 组合（n^2 + n^2)

所以时间复杂度对半开之后也只有n^2，可以接受。思路是建立哈希表。（因为并不要求去重，且只用算有多少种结果，所以可以直接用哈希）

```
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        hashmap = dict()
        for i in nums1:
            for j in nums2:
                if not (i+j) in hashmap:
                    hashmap[i+j] = 1
                else:
                    hashmap[i+j] += 1
        
        res = 0
        for m in nums3:
            for n in nums4:
                key = 0 - m - n
                if key in hashmap:
                    res += hashmap[key]
        
        return res
```

## 383. 赎金信
Counter的大成功，时间和空间复杂度O(m+n)
```
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        count_m = collections.Counter(magazine) # count_m is a dict to count all the letters in m
        count_r = collections.Counter(ransomNote)
        for key in count_r:
            if count_r[key] > count_m[key]:
                return False
        return True
```
因为只有26个字母，所以可以用数组实现哈希表，时间复杂度还是O(m+n)，但空间复杂度就是O(1)了。
```
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        ransom_count = [0] * 26
        magazine_count = [0] * 26
        for i in ransomNote:
            ransom_count[ord(i) - ord('a')] += 1
        for j in magazine:
            magazine_count[ord(j) - ord('a')] += 1
        return all(ransom_count[i] <= magazine_count[i] for i in range(26))
```


## 15. 三数之和

要求去重的话，用哈希表会比较麻烦，可以for loop一遍a，然后用左右两个指针从两边夹到中间。（先排序再双指针，探测到马上出现相同数字的时候，就直接移动指针跳过，从而去重）

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort() #[-4,-1,-1,0,1,2] #双指针的前提是有序
        res = []
        for i in range(len(nums)): # 用for loop遍历a
            if nums[i] > 0:
                break # 剪枝（注意这里是target为0特有的，四数之和就不能这么剪。因为target可能是负数-5，[-4,-1,0,0]就是符合的，如果用if nums[i] > target就会漏掉这种情况。四数之和，要剪枝的话只能这么剪：if nums[i] > target and target > 0 and nums[i] > 0: break
            if i > 0 and nums[i] == nums[i-1]: #剪枝a（这里就是去重操作了）：如果两个target是一样的，就直接后一步
# 请注意！！！！这里一定是if nums[i] == nums[i-1]，而不是if nums[i] == nums[i + 1]：如果写成后一种，你会直接跳过组内重复数字，如[-1,-1,2]。
                continue
            left = i + 1
            right = len(nums) - 1
            while left < right:
                if nums[left] + nums[right] > (0 - nums[i]):
                    right -= 1
                elif nums[left] + nums[right] < (0 - nums[i]):
                    left += 1
                else:
                    res.append([nums[i], nums[left], nums[right]])
                    while left < right and nums[left] == nums[left + 1]: #剪枝bc：一样的话就继续移动。注意这里的left < right不能省略：你可能会觉得外面那个大while不是已经规定了left < right吗？但别忘了，你上面几行还在left++ right--呢。所以要重新说一遍
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    right -= 1
                    left += 1
        return res
```

剪枝a的目的是防止第一个数重复，而剪枝bc的目的是在找到一个有效的三元组后防止第二个和第三个数重复。这两种剪枝策略都是为了防止重复的解，但由于它们在循环中的位置不同，所以它们的检查条件也不同。


