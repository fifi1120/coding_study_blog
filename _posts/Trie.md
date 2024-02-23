又叫prefix tree、字典树，是多叉树。

root节点是空的（像dummy node）。
<img width="584" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/e8a40d36-9063-41f3-b8dd-14f777a8dfa9">
<img width="395" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/0eb19290-5844-4c1c-abe4-fc51c261a677">

use cases：自动补全、拼写检查（模糊查找)、IP路由(前缀查找匹配）、
打字预测、词频统计（没有hashmap那么费空间，
如apple和apples基本是相同路径；但是hashmap里得存成俩个）。

模板：
常用method：
addWord、search、searchPrefix

```
class TrieNode:
    def __init__(self):
        self.children = [None] * 26
        self.isWord = False

class Trie:

    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            index = ord(char) - ord('a')
            if not node.children[index]:
                node.children[index] = TrieNode()
            node = node.children[index]
        node.isWord = True

    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            index = ord(char) - ord('a')
            if not node.children[index]:
                return False
            node = node.children[index]
        return node.isWord

    def starts_with(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            index = ord(char) - ord('a')
            if not node.children[index]:
                return False
            node = node.children[index]
        return True
```

Bitwise Trie: 2叉树，只存0或1.

<img width="1018" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/4a93fc3b-302b-49bb-b969-182ba7491ea8">

### 复杂度

#### 时间：

n个单词，每个单词的长度为k，那么构建字典树的时间复杂度是nk（字母的总数量）；

查找一个单词的时间复杂度是k（word长度）。

因为不管怎么样，你建立树、查找树，该走的路径都要走一遍。

#### 空间：

字典树里面的每个节点都需要26个子节点去存储指针（多叉树嘛），其实导致空间复杂度比较高，因为即使我只有1个子节点，我当时建立也是按照26个字母建立的array。

优化：

1. 把26个字母的array，变成hashmap（如果冲突，挂链表-可能变慢；或者挂红黑树）。
2. 如果末尾多次只有一个子节点，可以合并。如-s-i-a-b，我直接写成-siab，但是会加大编码难度。

思考题：如果删除怎么写？
