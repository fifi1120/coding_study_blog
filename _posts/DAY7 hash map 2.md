##  454.四数相加II 

这道题就是两两（n^2) 组合（n^2 + n^2)

所以时间复杂度对半开之后也只有n^2，可以接受。思路是建立哈希表。

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
