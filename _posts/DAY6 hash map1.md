非常重要的一句话：

#### 要快速判断一个元素是否出现集合里的时候，就要考虑哈希法（哈希表也叫散列表）。



为什么哈希表快：

我如果要在一个数组中找到某个数，我就需要从头到尾loop一遍，依次和每个loop到的数字对比，比较n次，这就是O(n)；

但是，如果我知道了他的索引，通过索引去找数字，那就是O(1)，因为计算机找索引只需要一步。

而哈希表就类似于已知索引（用特殊算法去规定索引），所以速度也是O(1)。


## 242. Valid Anagram

经典题 用Counter真的很好做.
```

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        s_count = collections.Counter(s)
        t_count = collections.Counter(t)
        if s_count == t_count:
            return True
        else:
            return False
```

方法二：直接用数组：
```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        count = [0] * 26
        for i in s:
            count[ord(i) - ord("a")] += 1
        for j in t:
            count[ord(j) - ord("a")] -= 1
        for x in count:
            if x == 0:
                continue
            else:
                return False
        return True
```

Counter函数和数组比较：

时间复杂度都是O(m+n)，但是用数组的空间复杂度只要O(1)（固定长度26），但是Counter的空间复杂度是O(m+n).

## 349. Intersection of Two Arrays

Counter太爽咯，时间和空间复杂度都是O(m+n)

```
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res = []
        count1 = collections.Counter(nums1)
        count2 = collections.Counter(nums2)
        for i in count1:
            if i in count2:
                res.append(i)
        return res
```

用数组，时间复杂度O(m+n)，空间复杂度O(1)

```
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        count1 = [0]*1001
        count2 = [0]*1001
        result = []
        for i in range(len(nums1)):
            count1[nums1[i]]+=1
        for j in range(len(nums2)):
            count2[nums2[j]]+=1
        for k in range(1001):
            if count1[k]*count2[k]>0:
                result.append(k)
        return result
```

还有Python set的bug方法：时间和空间复杂度上都是O(N+M)


```
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))

```

## 202 快乐数  

有一点需要想到的是：如果有重复（循环）就是永远也不会为1，要return True了。那怎么找到是否有重复值出现呢？（找某个数是否出现过）用set！

用helper函数会更清晰。

```
class Solution:
    def isHappy(self, n: int) -> bool:
        record = set()
        record.add(n)
        sum_happy = n
        while sum_happy != 1:
            sum_happy = self.calculation(sum_happy)
            if sum_happy in record:
                return False
            else:
                record.add(sum_happy)
        return True
    
    def calculation(self, n: int) -> int:
        res = 0
        for i in str(n):
            res += int(i) ** 2
        return res
```

## 1. 2Sum 哈希字典方法可以常做

这道题怎么想思路：因为要return index，还可能有重复数字，所以一定要用enemurate。

我自己先没用enemurate，直接自己设定好了一个map，再在里面去搜索，但这样的时间复杂度很高（已知value去求index需要O(n)。这是我的低效写法：

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
 
        map = {}
        for i in range(len(nums)):
            map[nums[i]] = target - nums[i]

        for index, value in enumerate(nums):
            if map[value] in nums[index + 1::]:
                return [index, nums.index(map[value], index + 1)]


```

这是比较好的写法，时间复杂度是O(n)。有两个值，用数组和set都不适用，可以用map（也就是字典）

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
       # dict里面装已经遍历过的元素，每次都判定一下<在已遍历的group里面有没有能和手头上这个match的> 这样就只用loop一遍了，不需要lst.index(value)增加一倍的时间复杂度。
       record = dict() #建立一个空字典,key是值，value是索引 -- 为什么要这样？因为我之后只能用key去查看是否存在于record字典呀
       for index, value in enumerate(nums):
            if target - value in record:
               return [index, record[target - value]]
            else:
                record[value] = index
```

