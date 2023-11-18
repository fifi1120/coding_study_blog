一、常考的几类二叉树

1. 满二叉树：所有点都是满的
```
        A
       / \
      B   C
     / \ / \
    D  E F  G
```

节点数量：2^k-1 =（k是深度）


2. 完全二叉树：除了底层，其它层都是满的 + 底层从左到右是连续的
```
        A
       / \
      B   C
     / \ 
    D   E
```
堆就是一个完全二叉树，同时保证父子节点的顺序关系。


3. 二叉搜索树（节点便于搜索，搜索的时间复杂度LogN-有序的）：

左边的所有节点值 < 中间 < 右边 （子树也满足规律）。


4. 平衡二叉搜索树AVL：

左右子树高度相差不能超过1。（子树也满足规律）

C++的map set nultimap multiset的底层实现都是平衡二叉树，所以用这些结构去插入或查询某个元素，是logN级别的。
- map和set里面的元素是有序的。


二、二叉树的存储方式

1. 链式存储：节点元素+左指针+右指针，常见。
面试可能要自己写constructor，构造二叉树 - 理解它是一个链表，只是有2个指针。用指针连起来之后，把头结点传入功能函数就可以了。

2. 线式存储：用数组保存二叉树：给节点标注下标。
 比如，下标i的左孩子是：2*i+1 右孩子2*i+2。


三、二叉树的遍历（要非常熟练）

1. DFS：一般用递归实现，分前中后序。（也可以用iterate，但考得比较简单）
一条路跑到黑，再回退一点换个方向，再一条路跑到黑。
前中后序：中部节点的位置。（如，前序：中部节点-左子树-右子树）

2. BFS：一层一层（一圈一圈），一般是层序遍历。


四、二叉树的定义

一定要自己熟练默写出来！！！

```python
#首先构造一个单独二叉树的节点（包括三部分，自己的值+左边指针连None+右边指针连Node)。
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val #这个val可以是int也可以是string，只是节点的内容值而已
            self.left = left #这里的left和right其实是TreeNode实例。
            self.right = right

#实现一般就是：
nodeB = TreeNode("B")或nodeB = TreeNode(1)

```



### 建造一个二叉树的方法就是 

step1 设立左右子树都是None的节点

```
class TreeNode:
        def __init__(self, val, left = None, right = None):
                self.val = val #这个val可以是int也可以是string，只是节点的内容值而已
                self.left = left #这里的left和right其实是TreeNode实例。
                self.right = right
```

step 2 把节点用链表的方式（比如nodeA.left = nodeB）连起来



