# 常考的几类二叉树

## 满二叉树：所有点都是满的
```
        A
       / \
      B   C
     / \ / \
    D  E F  G
```
节点数量：2^k-1 =（k是深度）


## 完全二叉树：要满足2个条件：除了底层，其它层都是满的 + 底层从左到右是连续的
```
        A
       / \
      B   C
     /  
    D  
```
<img width="633" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/ecc97081-b4f3-4f00-88d5-074d1fdf315f">

比如优先级队列其实是一个堆，堆就是一棵完全二叉树，同时保证父子节点的顺序关系。


## 二叉搜索树（节点便于搜索，是因为节点数值满足规律。搜索的时间复杂度LogN）：

左子树 < 中间节点 < 右子树 （注意是整颗子树也一定要满足规律！！！不仅仅只看左孩子和右孩子）。

<img width="642" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/7ad53cbf-5074-4a4a-82c7-a6ea0f93ef1d">

## 平衡二叉搜索树AVL（Adelson-Velsky and Landis）：

左右子树高度相差不能超过1。（子树也满足规律）

<img width="644" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/fa1cfac4-b881-480a-8f33-f9123ca9043a">



# 二叉树的存储方式

### 链式存储：节点元素+左指针+右指针，常见。

面试可能要自己写constructor，构造二叉树 - 理解它是一个链表，只是有2个指针。用指针连起来之后，把头结点传入功能函数就可以了。

<img width="591" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/5e6c83e9-91d6-4991-b04b-46717af25c02">

### 线式存储：用数组保存二叉树：给节点标注下标。（不常见）

<img width="605" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/d62999e0-d80a-4afa-ab62-d6ebb4fc0671">



# 二叉树的遍历（要非常熟练）

## DFS：一条路跑到黑，再回退一点换个方向，再一条路跑到黑。（分前中后）

一般DFS用递归实现。（基础不好的话，iterate法可以先放过）

前序、中序、后序：指的是中间节点的遍历顺序。

比如

前序（中间节点在最前）：中节点左子树右子树。中序：左子树中节点右子树。后序：左子树右子树中节点。



### 代码思路：

递归三部曲：

STEP 1: 确定递归函数的参数和返回值: 一般参数是root（TreeNode类型），返回的是list。（root就是你把哪个节点视作根。“root具体指代哪个节点”是会在递归中一次又一次地变化的：最开始的root是整颗大树的根节点，然后“root”又会变成root.left；root.right...不断递归下去）

STEP 2: 确定终止条件：当前遍历的”根”root是空了，那么本层递归就要结束了。

STEP 3: 确定单层递归的逻辑：

首先是left不断递归到底：left = self.preorderTraversal(root.left) # 一旦碰到调用了自己，一定要在一边不断“影分身”运行，直到完全运行完了，才能到下一行right的。这一行运行完的结果会是left = [4,1,2]。

然后是right不断递归到底：right = self.preorderTraversal(root.right) # “影分身”运行后，得到的是right = [6,7,8]

最后看是什么顺序combine：比如前序（中左右），就是[root.val]+left+right按这样的顺序组成list。# 最后就是return [5] + left即[4,1,2] + right即[6,7,8]。

<img width="637" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/db5821e7-9706-4453-81a8-3860aca25e69">

```
关于具体是怎么递归运行的：
XXX(5):
    left = XXX(4) 碰到函数就走不动道了，一定要运行完了再说
        left = XXX(1)
            left = XXX(None), return [] 其实按照路径是先走到最底部的，之后最后回过来return存入的时候，会把root存在前面而已。
【这就是为什么前中后序的代码 只有最后一句-存入的时候 root的位置不一样，因为路径都是一样的-先走到最底部-毕竟DFS】
            right = XXX(None), return []
            return [1]+[]+[] -> [1]
        right = XXX(2) 
            left = XXX(None), return []
            right = XXX(None), return []
            return [2]+[]+[] -> [2]
        return [4] + [1] + [2] --> [4,1,2] 一直运行到97行，存入left = [4,1,2]之后，才能进入到99行

    right = XXX(6)  --> [6,7,8]

    return [5] + [4,1,2] + [6,7,8]
```      

#### 具体代码-前序：
```
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        left = self.preorderTraversal(root.left)
        right = self.preorderTraversal(root.right)

        return  [root.val] + left +  right
```
#### 具体代码-中序：
```
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        left = self.inorderTraversal(root.left)
        right = self.inorderTraversal(root.right)

        return left + [root.val] + right
```
#### 具体代码-后序：
```
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        left = self.postorderTraversal(root.left)
        right = self.postorderTraversal(root.right)

        return left + right + [root.val]
```

## BFS：一层一层（一圈一圈）去遍历。


# 二叉树的定义

白板题常见，一定要自己熟练默写出来！！！

```python
# STEP 1: 构造一个单独二叉树的节点（包括三部分，自己的值 + 左边指针连None + 右边指针连Node)。
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val #这个val可以是int也可以是string，只是节点的内容值而已
        self.left = left #这个left是节点类型，比如在java里面这里就会写成TreeNode left。当然这里left和right会被默认设置成None。
        self.right = right

# STEP 2: 实现节点（把节点实例化）：
nodeB = TreeNode("B")或nodeB = TreeNode(1)

# STEP 3: 把节点们连起来，以下几种形式都可以的：
root.left = nodeB
root.right = TreeNode("happy")
nodeB.left = TreeNode(5)
TreeNode(3).right = TreeNode(8)
```

### 226.翻转二叉树
遍历的过程中去翻转每一个节点的左右孩子就可以达到整体翻转的效果。

注意只要把每一个节点的左右孩子翻转一下，就可以达到整体翻转的效果

这道题目使用前序遍历和后序遍历都可以，唯独中序遍历不方便，因为中序遍历会把某些节点的左右孩子翻转了两次！建议拿纸画一画，就理解了

那么层序遍历可以不可以呢？依然可以的！只要把每一个节点的左右孩子翻转一下的遍历方式都是可以的！
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return root
        root.left, root.right = root.right ,root.left
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        return root
```
### 101. 对称二叉树
有一点很重要：我们要比较的是两棵树，而不是简单地比较左右节点的值。

<img width="405" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/78380bf4-0718-47f6-9283-e36e161697fb">

比如这个图中，左边的2节点的左节点是3，右边的2节点的左节点是4，所以不能 if 左节点==左节点 and 右节点==右节点；而应该是：if 外侧节点==外侧节点 and 内侧节点==内侧节点

这道题的遍历方式最好用后序遍历（左右中），因为需要先判断左右子树是否对称，然后才能判断中间根节点代表的那个小数是否对称。
```
判断2个root开头的小树是否镜像的步骤相当于：

1.先看root，看几个方面：左右孩子是不是要存在都存在，要不存在都不存在；root的值是不是相等。
2.递归outside：左边树的左孩子 是否== 右边树的右孩子
3.递归inside：左边树的右孩子 是否== 右边树的左孩子
4.对小树下结论：（因为root已经知道val相等了，不然在第一步就已经return False）看outside and inside是否为True。
```

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        return self.compare(root.left, root.right)

    def compare(self, left, right):
        if left == None and right != None:
            return False
        elif left != None and right == None:
            return False
        elif left == None and right == None: 
            return True
        elif left.val != right.val: #这句话要写在最后，因为前面要排除left或者right是None的情况，不然会报错
            return False
        
        outside = self.compare(left.left, right.right)
        inside = self.compare(left.right, right.left)
        res = outside and inside
        return res
```

### 和101类似的题： 100.相同的树
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if p == None and q != None:
            return False
        elif p != None and q == None:
            return False
        elif p == None and q == None:
            return True
        elif p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

### 和101类似的题： 572.另一个树的子树
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not root and not subRoot:
            return True
        if not root:
            return False
        if self.compare(root, subRoot):
            return True
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot) #主函数的递归

    def compare(self, p, q):
        if p == None and q == None:
            return True
        elif p == None and q != None:
            return False
        elif p != None and q == None:
            return False
        elif p.val != q.val:
            return False
        return self.compare(p.left, q.left) and self.compare(p.right, q.right) #helper函数的递归
```

