如果一句话的库函数就可以解决，那么除了这样解决，你还要掌握另一种老老实实的做法。

## 344.反转字符串

简单一句话：
```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s.reverse() 
        return s
```

正常做法就是双指针
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

