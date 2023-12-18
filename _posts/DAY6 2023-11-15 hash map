非常重要的一句话：

要快速判断一个元素是否出现集合里的时候，就要考虑哈希法（哈希表也叫散列表）。



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
