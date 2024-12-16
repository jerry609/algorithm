![image.png](https://obsidian-1311563466.cos.ap-guangzhou.myqcloud.com/baguwen/20241216153831.png)

[Trie](https://baike.baidu.com/item/%E5%AD%97%E5%85%B8%E6%A0%91/9825209?fr=aladdin)**（发音类似 "try"）或者说 前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 Trie 类：
- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false`。

**示例：**

**输入**
$["Trie", "insert", "search", "search", "startsWith", "insert", "search"]$
$[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]$
**输出**
[null, null, true, false, true, null, true]

**解释**
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True

**提示：**
- `1 <= word.length, prefix.length <= 2000`
- `word` 和 `prefix` 仅由小写英文字母组成
- `insert`、`search` 和 `startsWith` 调用次数 **总计** 不超过 `3 * 104` 次

## 思路

假设字符串里面只有 a 和 b 两种字符。
- insert：例如插入字符串 aab，相当于生成了一条移动方向为「左-左-右」的路径。标记最后一个节点为终止节点。再插入字符串 aabb，相当于生成了一条移动方向为「左-左-右-右」的路径。标记最后一个节点为终止节点。
- search：例如查找字符串 aab，相当于查找二叉树中是否存在一条移动方向为「左-左-右」的路径，且最后一个节点是终止节点。
- startsWith：例如查找前缀 aa，相当于查找二叉树中是否存在一条移动方向为「左-左」的路径，无其他要求。

![image.png](https://obsidian-1311563466.cos.ap-guangzhou.myqcloud.com/baguwen/20241216151547.png)

推广到 26 种字母，其实就是一棵 26 叉树，对于 26 叉树的每个节点，可以用哈希表，或者长为 26 的数组来存储子节点。

算法

- 初始化：创建一棵 26 叉树，一开始只有一个根节点 root。26 叉树的每个节点包含一个长为 26 的儿子节点列表 son，以及一个布尔值 end，表示是否为终止节点。

- insert：
1. 遍历字符串 word，同时用一个变量 cur 表示当前在 26 叉树的哪个节点，初始值为 root。
2. 如果 word[i] 不是 cur 的儿子，那么创建一个新的节点 node 作为 cur 的儿子。如果 word[i]=a，那么把 node 记录到 cur 的 son[0] 中。
3. 如果 word[i]=b，那么把 node 记录到 cur 的 son[1] 中。依此类推。
4. 更新 cur 为儿子列表中的相应节点。遍历结束，把 cur 的 end 标记为 true。

- search 和 startsWith 可以复用同一个函数 find：
1. 遍历字符串 word，同时用一个变量 cur 表示当前在 26 叉树的哪个节点，初始值为 root。
2. 如果 word[i] 不是 cur 的儿子，返回 0。search 和 startsWith 收到 0 之后返回 false。
3. 更新 cur 为儿子列表中的相应节点。
4. 遍历结束，如果 cur 的 end 是 false，返回 1，否则返回 2。search 如果收到的是 2，返回 true，否则返回 false。startsWith 如果收到的是非 0 数字，返回 true，否则返回 false。

```c++
struct Node{
	Node* son[26]{};
	bool end = false;
}
class Trie {
	Node* root = new Node();
	int find(string word){
		Node* cur = root;
		for(char c: word){
			c- = 'a';
			if(cur->son[c]==nullptr){
				return 0;
			}
			cur = cur->son[c];
		}
		return cur->end?2:1;
	}
public:
    void insert(string word) {
		Node* cur = root;
		for(char c : word){
			c -= 'a';
			if(cur->son[c] == nullptr){
				cur->son[c] = new Node();
			}
			cur = cur->son[c];
		}
		cur->end = true;
    }

    bool search(string word) {
		return find(word)==2;
    }

    bool startsWith(string prefix) {
		return find(prefix)!=0;
    }

};

  

/**

 * Your Trie object will be instantiated and called as such:

 * Trie* obj = new Trie();

 * obj->insert(word);

 * bool param_2 = obj->search(word);

 * bool param_3 = obj->startsWith(prefix);

 */
```