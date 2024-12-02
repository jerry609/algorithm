[[19 删除链表的倒数第 N 个结点]]
![image.png](https://obsidian-1311563466.cos.ap-guangzhou.myqcloud.com/baguwen/20241130094102.png)

1. 暴力
2. 快慢指针


## 总结

1. 暴力
错误代码
```c++
/**

 * struct ListNode {

 *  int val;

 *  struct ListNode *next;

 *  ListNode(int x) : val(x), next(nullptr) {}

 * };

 */

class Solution {

public:

    /**

     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可

     *

     *

     * @param pHead ListNode类

     * @param k int整型

     * @return ListNode类

     */

    ListNode* FindKthToTail(ListNode* pHead, int k) {

        ListNode* cur = pHead;

        int c = 0;

        while(cur){

            c++;

            cur = cur->next;

        }

        cur = pHead;

        int n = c-k+1;

        while(cur){

            cur = cur->next;

            c--;

            if (c==n) {

                return cur;

            }

        }

        return nullptr;

    }

};
```

- 没有考虑如果k>c怎么办
- 循环条件也不够简单，可以直接while(n) n--

2. 一次扫描，双指针
双指针的经典应用，如果要删除倒数第n个节点，让fast移动n步，然后让fast和slow同时移动，直到fast指向链表末尾。删掉slow所指向的节点就可以了。

```c++
/**
 * struct ListNode {
 *	int val;
 *	struct ListNode *next;
 *	ListNode(int x) : val(x), next(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    ListNode* FindKthToTail(ListNode* pHead, int k) {
        if (pHead == nullptr || k <= 0) {
            return nullptr;
        }

        ListNode* fast = pHead;
        ListNode* slow = pHead;

        // 将 fast 指针向前移动 k 步
        for(int i = 0; i < k; ++i){
            if (fast == nullptr) {
                return nullptr; // k 大于链表长度
            }
            fast = fast->next;
        }

        // 同时移动 fast 和 slow 指针，直到 fast 到达链表末尾
        while(fast != nullptr){
            fast = fast->next;
            slow = slow->next;
        }

        return slow; // slow 指向倒数第 k 个节点
    }
};

```