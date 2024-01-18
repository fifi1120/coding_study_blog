### 669. 修剪二叉搜索树

我这辈子第一次这么清楚地明白了递归信仰之跃，。。。。。。。
这道题可以常常复习，复习就是理解递归的本质。

```
class Solution:
    def trimBST(self, root: TreeNode, low: int, high: int) -> TreeNode:
        if root is None:
            return None # 这两句话在修建二叉树里面是固定的，到叶子了就直接结束本次递归了
        if root.val < low: # 如果当前root比界限更小，要做的事情：删除这个节点 - 二叉树的所谓”删除“就是让root.left/right = 跳过<要被删的节点>的别的节点，即新上位的节点。那如果这个节点因为太小了要被删除，说明要上位的节点可能存在被删除节点的右侧，所以这里return root.right。但这里又不能直接让右节点上位（因为我并不知道是否符合条件），所以这里可以直接来个递归，以此return在右子树中符合条件的那个节点
            return self.trimBST(root.right, low, high)
            # 重要错误：我之前写【root.left = self.trimBST(root.right, low, high) 】我我这里想表达的是“root的father的left = self.trimBST(root.right, low, high)“，但是在树里面倒着找father是不可用的。你一般只能先return一个东西返回给上一层，后面再找机会链接某root.left = return的东西
            # 这句话return self.trimBST(root.right, low, high) 同时做了两件事情：1. 修剪root.right这颗小子树底下所有的不符合条件的点 2. return root.right这颗小子树修剪后的最高点。
            # 为什么1可以实现呢？我写的时候，是直接相信这个递归可以帮我修剪，所以直接return的吗？--- 事实上，递归的工作方式确实需要一定程度的“信任”或“假设”：您需要假设递归调用能够正确地完成其任务。这是递归的一个关键概念，称为递归信任假设（recursive leap of faith）。信任递归就像信任数学归纳法。--- 那你就要想了，这个xxx递归方程总的来说会return什么呢？会return修剪后的树的头节点。
        if root.val > high:
            return self.trimBST(root.left, low, high)

        root.left = self.trimBST(root.left, low, high)  # root.left 接住 修剪后的左子树的最高节点
        root.right = self.trimBST(root.right, low, high)

        return root
```


### 108.将有序数组转换为二叉搜索树
 
这道题是二分法+recursion的结合。

原本的While Left <= right会变成if left < right: return

```
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        return self.binaryBuild(nums, 0, len(nums)-1)

    def binaryBuild(self, nums, left_index, right_index):
        if left_index > right_index:
            return
        mid = left_index + (right_index - left_index)//2
        root = TreeNode(nums[mid])
        root.left = self.binaryBuild(nums, left_index, mid-1) # 相信binaryBuild(nums, left_index, mid-1)会return 用左半部分list 构建好的 平衡二叉树
        root.right = self.binaryBuild(nums, mid+1, right_index)
        return root
```

### 538.把二叉搜索树转换为累加树

和DFS6的两道双指针题目类似，这里也用到了双指针cur（节点），pre_val（int）。

```
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.pre_val = 0  # pre一定得单独拎出来，而不能写在递归函数里面，因为这里相当于pre指针需要是全局的，写在递归函数每次递归一次都会被更新的。。。
        self.traversal(root)
        return root

    def traversal(self, cur): # 这个函数的目的就是in place修改树的值，右中左
        if cur is None:
            return    
        self.traversal(cur.right) # 右
        cur.val += self.pre_val 
        self.pre_val = cur.val # 中
        self.traversal(cur.left) # 左
```
