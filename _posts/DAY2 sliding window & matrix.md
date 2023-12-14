# 普通双指针

# 滑动窗口：

一定记得是right指针用for loop依次往右移，左边指针【根据题目意思】来看动不动。

因为只有这样才能work。如果是left指针依次移动，right的loop方式还是时间复杂度很高，因为你不知道应该在哪里结束（右边没有限制，你想检查必须得检查到最末尾一个元素）；

但是如果是right每次往右移动一个，我再去判定left怎么移动就比较简单了，因为至少永远是在一个有限的范围内。

所以滑动窗口必须记住：

```
for loop 右指针 依次右移:

   while 题目条件：

     左指针右移
```

这里可以设置一个变量记录最大值等等。




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


