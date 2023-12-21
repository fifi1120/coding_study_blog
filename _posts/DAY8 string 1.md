如果一句话的库函数就可以解决，那么除了这样解决，你还要掌握另一种老老实实的做法。

## 344.反转字符串

简单一句话：（时间复杂度是O(n)，空间复杂度是O(1))
```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s.reverse() 
        return s
```

正常做法就是双指针（时间复杂度是O(n)，空间复杂度是O(1))
```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left] # 第一次自己写的时候忘记交换后移动指针了，这是一个非常常见的错误
            left += 1
            right -= 1
        return s
```

## 541. 反转字符串II

所以当需要固定规律一段一段去处理字符串的时候，要想想在在for循环的表达式上做做文章。（跳格）
### 方法一：

先string变list，然后用index直接操作：s_lst[left], s_lst[right] = s_lst[right], s_lst[left] （一定要先变成list，因为string是不能inplace修改的）

```
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        s_lst = list(s)  # 一开始就将字符串转换为列表
        for left in range(0, len(s), 2 * k):
            # 计算右指针，确保不越界
            right = min(left + k - 1, len(s) - 1)
            # 反转指定部分
            while left < right:
                s_lst[left], s_lst[right] = s_lst[right], s_lst[left]
                left += 1
                right -= 1
        return ''.join(s_lst)  # 将列表转换回字符串
```

# 插播一个必须熟悉背诵的string和list转换：
## string变list：lst = list(string)
## list变string：''.join(lst)

### 方法二：

直接切片

```
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        # Two pointers. Another is inside the loop.
        p = 0
        while p < len(s):
            p2 = p + k
            # Written in this could be more pythonic.
            s = s[:p] + s[p: p2][::-1] + s[p2:]
            p = p + 2 * k
        return s
```

## 151.翻转字符串里的单词

比较好的综合题，我之前忘记了split()的妙用。用split把string换成列表：无论中间有多少个空格，split() 都会把它们当作一个分隔符来处理。

```
class Solution:
    def reverseWords(self, s: str) -> str:
        res_lst = s.split() # O(n)
        res_lst.reverse() # O(n)
        return ' '.join(res_lst)
```



