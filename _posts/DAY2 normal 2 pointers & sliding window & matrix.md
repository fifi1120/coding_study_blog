# 普通双指针

## LC  977.有序数组的平方 

两个指针，一前一后，逐渐移动到中间。从前往后/从后往前双指针。为什么想到用双指针呢？

数组其实是有序的， 只不过负数平方之后可能成为最大数了。

那么数组平方的最大值就在数组的两端，不是最左边就是最右边，不可能是中间。

此时可以考虑---->双指针法了，i指向起始位置，j指向终止位置。


```
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        left = 0
        right, i = len(nums) - 1, len(nums) - 1 # 提前定义i是很重要的一步。我之前用res.insert(0, something)直接再乘以了一个O(n)。有了i之后直接用index去改，就只是O(1)。
        res = [0] * len(nums) # 初始化成任意数不影响的。你用了i就要提前定义好res。
        while left <= right:
            if abs(nums[left]) <= abs(nums[right]):
                res[i] = nums[right] * nums[right]
                right -= 1
                i -= 1
            else:
                res[i] = nums[left] * nums[left]
                left += 1
                i -= 1
        return res
```

## 自选扩展题 167. Two Sum II - Input Array Is Sorted

很简单，哪个菩萨出的题。从前往后/从后往前双指针。

```
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:

        left = 0
        right = len(numbers)-1
        while left < right:
            if numbers[left] + numbers[right] > target:
                right -= 1
            elif numbers[left] + numbers[right] < target:
                left += 1
            else:
                return [left+1, right+1]

```

## 自选扩展题 15. 3Sum

其实就是for+从前往后/从后往前双指针；就是很难想到需要用双指针，哈哈。

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort() #[-4,-1,-1,0,1,2]
        res = []
        for i in range(len(nums)): # target = -nums[i]
            if i > 0 and nums[i] == nums[i-1]: #降低一点时间复杂度，如果两个target是一样的，就直接后一步
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
                    while left < right and nums[left] == nums[left + 1]: #同理，也是，一样的话就继续移动，降低时间复杂度。注意这里的left < right不能省略：你可能会觉得外面那个大while不是已经规定了left < right吗？但别忘了，你上面几行还在left++ right--呢。所以要重新说一遍
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    right -= 1
                    left += 1
        return res

```

## 自选扩展题 344. Reverse String

难度在于in-place修改技巧，你要交换的话可以直接：s[left], s[right] = s[right], s[left]

```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left] #自己这里不会写
            left += 1
            right -= 1

```

# 滑动窗口：

一定记得是right指针用for loop依次往右移，左边指针【根据题目意思】来看动不动。

因为只有这样才能work。如果是left指针依次移动，right的loop方式还是时间复杂度很高，因为你不知道应该在哪里结束（右边没有限制，你想检查必须得检查到最末尾一个元素）；

但是如果是right每次往右移动一个，我再去判定left怎么移动就比较简单了，因为至少永远是在一个有限的范围内。

所以滑动窗口必须记住：

```
for loop 右指针 依次右移:

   根据条件移动左指针
   打擂台

```

这里可以设置一个变量记录最大值等等。

### 其实，滑动窗口的核心思想是：打擂台+右边依次动，左边看情况动不动。

## 209.长度最小的子数组


```
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        if sum(nums) < target:
            return 0
        
        res = len(nums) #cur=6
        
        left = 0
        cur_sum = 0 
        for right in range(len(nums)): 
            cur_sum += nums[right] #for loop每走一步，cur_sum每更新一次。
            while cur_sum >= target:
                res = min(res, right - left + 1)
                cur_sum -= nums[left] #左指针尝试向右移动一位，如果还符合条件（窗口内和大于等于target，就会再次进入while loop并且更新res，但是如果不满足条件，也就不会更新res，所以不用担心：即使这里l多走了一位，我的res还是准确的。
                left += 1
        return res

```

## 自选拓展题 3. Longest Substring Without Repeating Characters

也符合 【打擂台】 + 【for套while】 的模板：

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if s == "":
            return 0
        left = 0
        res = 1
        for right in range(1, len(s)): #right=1-7
            while s[right] in s[left:right]: #while s[1] in s[0:2]
                left += 1
            
            if right - left + 1 > res:
                res = right - left + 1
        return res

```

## 自选扩展题 424. 替换后的最长重复字符


其实就是维护一个滑动窗口，然后保证窗口大小减去出现次数最多的字符的字数之差小于等于k即符合，窗口可继续扩张，否则就不符合题意，右移左指针

```
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}
        left = 0
        max_count = 0
        res = 0
        for right in range(len(s)):
            count[s[right]] = 1 + count.get(s[right], 0)
            max_count = max(max_count, count[s[right]])
            while right - left + 1 - max_count > k: #不满足题目条件，left往右移动试试呢
                count[s[left]] -= 1
                left += 1
            res = max(res, right - left + 1)
        return res

```

## 自选扩展题 485. Max Consecutive Ones

简单题都要想一会儿。。。原来easy题才是我的归属

```
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        res = 0
        left = 0
        for right in range(len(nums)):
            if nums[right] == 0:
                left = right + 1
            if right - left + 1 > res:
                res = right - left + 1
        return res

```

## 自选拓展题 643. Maximum Average Subarray I

其实很简单，但是是滑动窗口的另一种做法，一定要动手做。max还是cur分清楚。
```
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        max_sum = sum(nums[0:k])  # 初始化最大和为前 k 个元素的和
        cur_sum = max_sum  # 当前窗口的和
        left = 0  # 初始化左指针

        for right in range(k, len(nums)):
            cur_sum += nums[right] - nums[left]  # 更新当前窗口的和
            max_sum = max(max_sum, cur_sum)  # 如果当前窗口的和更大，则更新最大和
            left += 1  # 移动左指针

        return round(max_sum / k, 5)


```


# Matrix

matrix题目要额外注意循环不变量的原则，坚持左闭右开就永远左闭右开。

## 59.螺旋矩阵II

题目建议：  本题关键还是在转圈的逻辑，在二分搜索中提到的区间定义，在这里又用上了。 
（确定左开右闭之后就保持不变，一直这样，不然很容易错的）

```
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)] # 初始话matrix，希望直接背下来
        startx, starty = 0, 0               # 起始点就是[0,0]
        loop, mid = n // 2, n // 2          
        # loop是会在矩阵中转几圈次（奇数偶数都适用），mid是矩阵的中心点（奇数n才有中心点）
        count = 1                           # 计数

        for offset in range(1, loop + 1) :      # 比如n=5,loop=2，offset=1,2。111 当offset=1; 底下的4个循环用的都是n-offset=4，每轮走4步，下面4轮循环的startx=starty=0的。；；；***当offset=2
            for i in range(starty, n - offset) :    # 从左至右，左闭右开：range(0, 5-1),i=0,1,2,3; 内层loop4次就是走4个格子; nums[0][0]=1; num[0][1]=2; nums[0][2]=3;nums[0][3]=4；；；***for i in range(1,5-2), i =1,2
                nums[startx][i] = count # 走第一行，startx保持不变
                count += 1 #这个for loop是在走矩阵的第一行（不含最后一个格子）：1 2 3 4。走完count=5 
            for i in range(startx, n - offset) :    # 从上至下，左闭右开：range(0, 5-1), i=0,1,2,3
                nums[i][n - offset] = count # 走最右边那一列，nums[0][4]=5;nums[1][4]=6;nums[2][4]=7;nums[3][4]=8;
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左：range(4, 0, -1).i=4,3,2,1
                nums[n - offset][i] = count # nums[4][4]=9; nums[4][3]=10; nums[4][2]=11; nums[4][3]=12;                
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上：range(4,0, -1)i=4,3,2,1
                nums[i][starty] = count #nums[4][0]=23; nums[3][0]=14; num[2][0]=15; nums[1][0]=16
                count += 1    #count = 17
            startx += 1         # 更新起始点
            starty += 1         # 走完4个for loop之后，startx=starty=1, 下一轮就是offset=2，n-offset=3，然后下一圈就是每次往前走2格。。。

        if n % 2 != 0 :			# 如果n是偶数，走完就可以可以直接return了；如果n是奇数，就是还有一个最中心点要填。
            nums[mid][mid] = count 
        return nums
```

## 类似题目：54.螺旋矩阵
### 表扬一下自己，是自己完全做出来的，最后有bug，用gpt debug，gpt真是蠢钝如猪啊！！！ 我感觉还是不能太依赖。对自己人脑的智慧有了更多的自信。

```
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        res = []
        startx, starty = 0,0
        loop = min(len(matrix)//2, len(matrix[0])//2) #loop=1
        #mid_length = max(len(matrix), len(matrix[0])) - loop * 2
        mid_start = loop

        #m是列数，4
        m = len(matrix[0])
        #n是行数，3
        n = len(matrix)

        for offset in range(1, loop+1): #offset=range(1,2) offset=1
            for i in range(starty, m-offset): #range(0,3),i=0,1,2
                res.append(matrix[startx][i]) #matrix[0][0];matrix[0][1];matrix[0][2]
            for i in range(startx, n-offset): #range(0,1),i=0,1
                res.append(matrix[i][m-offset])#matrix[0][3];matrix[1][3]
            for i in range(m-offset, starty, -1): #range(3,0,-1),i=3,2,1
                res.append(matrix[n-offset][i]) #matrix[2][3],matrix[2][2];matrix[2][1]
            for i in range(n-offset, startx, -1): #range(2, 0, -1),i=2,1
                res.append(matrix[i][starty])
            startx += 1
            starty += 1

        cur_len = len(res)
    
        
        if len(res) != m * n:
            if m < n:
                for _ in range(n-loop*2):
                    res.append(matrix[startx][starty])
                    startx += 1
            else:
                for _ in range(m-loop*2):
                    res.append(matrix[startx][starty])
                    starty += 1
        return res
```

我感觉matrix题就是不停地模拟和熟练

## 自选简单题也难以独立做出来哦： 566. Reshape the Matrix

```
class Solution:
    def matrixReshape(self, mat: List[List[int]], r: int, c: int) -> List[List[int]]:
        lst = []
        for m in range(len(mat)): 
            for n in range(len(mat[0])):
                lst.append(mat[m][n])
        res = []
        #重大问题：不能用res = [[0] * c] * r建立空白矩阵，这会导致所有行引用同一个列表，非要建立的话可以这么写：res = [[0 for _ in range(c)] for _ in range(r)]
        if r * c != len(lst): #illegal
            return mat
        else:
            for i in range(0, len(lst), c):  #i就是每隔column就再来一遍（有几行就遍历几次）
                res.append(lst[i:i+c])
            return res
        
```

这道题非常基础，就是先把矩阵变成一维的，再从一维的变成二维的。有一些特定的写法比如如何展开矩阵（2个for loop）希望你牢记在心，写的时候可以更快更有自信。

怎么把矩阵从一维重装成二维呢？就是
```
for i in range(0, len(lst), c): #按列跳步，循环行数次
    res.append(lst[i:i+c]) #把该行填满（填列数个元素）
```
                            
