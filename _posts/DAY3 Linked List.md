链表是不连续分布的，该存的都存了，不一样的链表其实是连接线的不一样。
<img width="487" alt="image" src="https://github.com/fifi1120/fifi1120.github.io/assets/98888516/2d10b81e-d204-4f66-9e69-45ca88eea8a2">

有些题目要手写链表的构造函数的：

```
class ListNode:
    def __init__(self, val, next=None):
        self.val = val #设定值，定义数据域
        self.next = next #设定连线，定义指针域
```



<img width="742" alt="image" src="https://github.com/fifi1120/fifi1120.github.io/assets/98888516/8cc8498a-ae91-498f-8bbe-eb4347b145d3">

因为插入删除的复杂度是O(1)，是因为链表的插入删除操作并不影响别的指针域或者数据域。

但链表只能从头尾开始走，所以查询要走O(n).


### 删除元素

## 203. Remove Linked List Elements

唯一需要的技巧是设置dummy

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head
        cur = dummy
        while cur.next is not None:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        return dummy.next
```


