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
