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

左边的所有节点值 < 中间 < 右边 （子树也满足规律）。

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

一般DFS用递归实现。（也可以用iterate，但考得比较简单）

前序、中序、后序：指的是中间节点的遍历顺序。

比如

前序（中间节点在最前）：中节点左子树右子树。中序：左子树中节点右子树。后序：左子树右子树中节点。
<img width="637" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/db5821e7-9706-4453-81a8-3860aca25e69">


### 代码具体：

递归三部曲：

STEP 1: 确定递归函数的参数和返回值: 一般参数是root（TreeNode类型），返回的是list。（root就是你把哪个节点视作根。“root具体指代哪个节点”是会在递归中一次又一次地变化的：最开始的root是整颗大树的根节点，然后“root”又会变成root.left；root.right...不断递归下去）

STEP 2: 确定终止条件：当前遍历的”根”root是空了，那么本层递归就要结束了。

STEP 3: 确定单层递归的逻辑：

首先是left不断递归到底：left = self.preorderTraversal(root.left) # 走到left的底部触及到None，然后return，退回上一步

然后是right不断递归到底：right = self.preorderTraversal(root.right) # 走到right的底部触及到None，然后return，退回上一步

最后看是什么顺序combine：比如前序（中左右），就是[root.val]+left+right按这样的顺序组成list。

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






