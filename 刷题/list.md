---
title: 链表
date: 2018-10-31 13:47:40
type: leetcode
categories: leetcode
cover: https://pic.leetcode-cn.com/e86e947c8b87ac723b9c858cd3834f9a93bcc6c5e884e41117ab803d205ef662-%E7%9B%B8%E4%BA%A4%E9%93%BE%E8%A1%A8.png
---

# 160. 相交链表
简单
相关标签
相关企业
给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

# 代码
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
            Set<ListNode> visited = new HashSet<ListNode>();
            ListNode temp = headA;
            while(temp != null){
                visited.add(temp);
                temp = temp.next;
            }
            temp = headB;
            while(temp != null){
                if(visited.contains(temp))   return temp;
                temp = temp.next;
            }
            return null;
    }
}
 ```

 # 206. 反转链表
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = new ListNode(-1);
        ListNode temp = head;
        while(temp != null){
            ListNode next  = temp.next;
            temp.next=pre;
            pre = temp;
            temp = next;
        }
        return pre;
    }
}
```

# 234. 回文链表
给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> valuse = new ArrayList<>();
        ListNode cur = head;
        while(cur != null){
            valuse.add(cur.val);
            cur = cur.next;
        }
         int left = 0;
         int right = valuse.size()-1;
         while(left < right){
             if(!valuse.get(left).equals(valuse.get(right)))
                return false;
            left++;
            right--;
         }
         return true;
    }
}

```

# 141. 环形链表
给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。

如果链表中存在环 ，则返回 true 。 否则，返回 false 。
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }
        ListNode fast = head, slow = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(slow == fast)
                return true;
        }
        return false;
    }
}
```

# 21. 合并两个有序链表
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

递归也可以解

 

示例 1：


输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
示例 2：

输入：l1 = [], l2 = []
输出：[]
示例 3：

输入：l1 = [], l2 = [0]
输出：[0]

```java/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode prehead = new ListNode(-1);
        ListNode prev = prehead;
        while(list1 != null && list2 != null){
            if(list1.val < list2.val){
                prev.next = list1;
                list1 = list1.next;
            }
            else{
                prev.next = list2;
                list2 = list2.next;
            }
            prev = prev.next;
        }
        prev.next = list2 == null? list1 :list2;
        return prehead.next;


    }
}
```

# 2. 两数相加

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0); // 创建一个虚拟头节点
        ListNode p = l1, q = l2, curr = dummyHead;
        int carry = 0; // 进位

        while (p != null || q != null) {
            int x = (p != null) ? p.val : 0;
            int y = (q != null) ? q.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;

            curr.next = new ListNode(sum % 10);
            curr = curr.next;

            if (p != null) p = p.next;
            if (q != null) q = q.next;
        }

        if (carry > 0) {
            curr.next = new ListNode(carry);
        }

        return dummyHead.next; // 返回虚拟头节点的下一个节点，即结果链表的头节点
    }
}

```

超时做法 链表中存储整数进行加减操作通常与我们常规计算加减法类似 低位在前反而利于加法的计算。下面这种方法一定会超时，在大数问题上存在太多转换
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverse(ListNode l){
        ListNode prehead = new ListNode (-1);
        ListNode  pre = prehead;
        while(l != null){
            ListNode next = l.next;
            l.next = pre;
            pre = l;
            l=next;
        }
        return pre;
    }
    public int listToInt(ListNode l1){
        if(l1 == null)  return 0;
        int a = 1,ans=0;
        while(l1 != null){
            ans += a * l1.val;
            a *= 10;
        }
        return ans;
    }
    public ListNode intToList(int ans){
        ListNode head = new ListNode(-1);
        ListNode prehead = head;
        int cur = 10;
        while(ans != 0){
            ListNode temp = new ListNode(ans%10);
            head.next = temp ;
            head=head.next;
            ans = ans /10;
        }
        return prehead;

    }
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        l1 = reverse(l1);l2 = reverse(l2);
        int num1,num2,ans;
        ans = listToInt(l1) + listToInt(l2);
        return intToList(ans);
    }
}
```

# 19. 删除链表的倒数第 N 个结点

提示
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode prehead =new ListNode(-1);
        prehead.next = head;
        int size = 0;
        ListNode temp = prehead;
        if(head.next ==null && n==1){
            return null;
        }
        while(temp.next!= null){
            size+=1;
            temp = temp.next;
        }
        temp=prehead;
        for(int i=0;i<size-n;i++){
            temp =temp.next;
        }
        if(temp.next.next == null){
            temp.next =null;
        }
        else{
            temp.next =temp.next.next;
        }
        return prehead.next;
    }
}
```

# 24. 两两交换链表中的节点

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

## 思路
比较简单的标记交换 注意交换的次序即可 画图可以辅助理解next之间的联系 注意哪些next需要保持 哪些需要更新

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode next = null;
        if(head == null)    return null;
        if(head.next == null)   return head;
        ListNode prehead = new ListNode(-1,head);
        ListNode temp = prehead;
        while(temp.next!=null && temp.next.next != null){
            ListNode  second =temp.next.next;
            ListNode first =temp.next;
            temp.next = second;
            first.next =second.next;
            second.next = first;
            temp= first;
        }
        return prehead.next;
    }
}
```

# 25. K 个一组翻转链表

给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

```
public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) {
            return head;
        }

        // 创建一个哑节点作为新链表的头部
        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        // 初始化指针
        ListNode pre = dummy;
        ListNode end = dummy;

        while (end.next != null) {
            // 移动 end 指针，找到当前要翻转的 k 个节点的末尾
            for (int i = 0; i < k && end != null; i++) end = end.next;
            if (end == null) break; // 如果节点不足 k 个，结束循环

            // 断开链表，并翻转这 k 个节点
            ListNode start = pre.next; // 翻转的起始节点
            ListNode next = end.next; // 保存下一段链表的起始节点
            end.next = null; // 断开链表
            pre.next = reverse(start); // 翻转链表，并连接到 pre 节点

            // 重新连接翻转后的链表
            start.next = next;
            pre = start; // 更新 pre 指针到翻转部分的末尾
            end = pre; // 重置 end 指针，准备下一次循环
        }

        return dummy.next;
    }

    // 辅助函数：翻转链表
    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
}

// 链表节点的定义
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

```

# 148. 排序链表
给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode merge(ListNode head1, ListNode head2){
        ListNode dummy = new ListNode(-1);
        ListNode temp = dummy, temp1 = head1, temp2 = head2;
        while(temp1 != null && temp2 != null){
            if(temp1.val <= temp2.val){
                temp.next = temp1;
                temp1 = temp1.next;
            }
            else{
                temp.next = temp2;
                temp2 = temp2.next;
            }
            temp = temp.next;
        }
        temp.next = temp1==null?temp2:temp1;
        return dummy.next;
    }
    public ListNode sortList(ListNode head) {
        if(head == null)    return head;
        int length = 0;
        ListNode node =head;
        while(node != null){
            node = node.next;
            length++;
        }
        ListNode dummy = new ListNode(0,head);
        for(int sublength = 1;sublength<length;sublength <<= 1){
            ListNode prev = dummy,curr = dummy.next;
            while(curr != null){
                ListNode head1 = curr;
                for(int i=1;i < sublength && curr.next != null;i++ ){
                    curr = curr.next;
                }
                ListNode head2 = curr.next;
                curr.next = null;
                curr = head2;
                for(int i=1;i < sublength && curr !=null &&curr.next != null;i++ ){
                    curr = curr.next;
                }
                ListNode next = null;
                if(curr != null){
                    next = curr.next;
                    curr.next =null; 
                }
                ListNode merged =merge(head1,head2);
                prev.next = merged;
                while(prev.next!= null){
                    prev = prev.next;
                } 
                curr = next;

            }
        }

        return dummy.next;
       
    }
}
```

## 思路
采用先导入数组的任何排序形式 时间复杂度最优为nlogn，需要优化为n下解决。

可以自顶向下解决，也就是归并排序，也可以自底向上解决，下面给出的是自底向上的解决方案

![](img/leetcode/leetcode_148.png)

# 23. 合并 K 个升序链表

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode ans = null;
        for(int i = 0;i < lists.length; i++){
            ans = mergeTwo(ans,lists[i]);
        }
        return ans;
    }
    public ListNode mergeTwo(ListNode a, ListNode b){
        if(a == null || b == null)
            return a != null ? a : b;
        ListNode head = new ListNode(0);
        ListNode tail = head ,pa = a, pb = b;
        while(pa != null && pb != null){
            if(pa.val < pb.val){
                tail.next = pa;
                pa = pa.next;
            }else{
                tail.next = pb;
                pb = pb.next;
            }
            tail = tail.next;
        }
        tail.next = (pa != null ? pa : pb);
        return head.next;
    }
}
```
方法：顺序合并
思路

我们可以想到一种最朴素的方法：用一个变量 ans 来维护以及合并的链表，第 i次循环把第 i 个链表和 ans 合并，答案保存到 ans 中。
复杂度分析

时间复杂度：假设每个链表的最长长度是 n。在第一次合并后，ans 的长度为 n；第二次合并后，ans 的长度为 2×n，第 i 次合并后，ans 的长度为 i×n。第 iii 次合并的时间代价是 O(n+(i−1)×n)=O(i×n)，那么总的时间代价为 O(∑i=1k(i×n))=O((1+k)⋅$k^2$×n) 
 ×n)=O($k^2$)，故渐进时间复杂度为 O($k^2$n)


空间复杂度：没有用到与 k 和 n 规模相关的辅助空间，故渐进空间复杂度为 O(1)。


# 146. LRU 缓存
已解答
中等
相关标签
相关企业
请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现 LRUCache 类：
LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。

 

示例：

输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4

```java
class LRUCache {
    class DLinkNode{
        int key;
        int value;
        DLinkNode prev;
        DLinkNode next;
        public DLinkNode(){};
        public DLinkNode(int _key, int _value){
            key = _key;
            value = _value;
        }
    }
    private Map<Integer,DLinkNode> cache = new HashMap<Integer,DLinkNode>();
    private int size;
    private int capacity;
    private DLinkNode head,tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        head = new DLinkNode();
        tail = new DLinkNode();
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        DLinkNode node =cache.get(key);
        if(node == null)    return -1;
        movetohead(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        DLinkNode node = cache.get(key);
        if(node == null){
            DLinkNode newnode = new DLinkNode(key,value);
            cache.put(key,newnode);
            addtohead(newnode);
            ++size;
            if(size > capacity){
                DLinkNode tail = removetail();
                cache.remove(tail.key);
                --size;
            }

        }else{
            node.value = value;
            movetohead(node);
        }
    }
    public void addtohead(DLinkNode node){
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }
    public void removenode(DLinkNode node){
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }
    public void movetohead(DLinkNode node){
        removenode(node);
        addtohead(node);
    }
    private DLinkNode removetail() {
        DLinkNode res = tail.prev;
        removenode(res);
        return res;
    }


}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

方法一：哈希表 + 双向链表
算法

LRU 缓存机制可以通过哈希表辅以双向链表实现，我们用一个哈希表和一个双向链表维护所有在缓存中的键值对。

双向链表按照被使用的顺序存储了这些键值对，靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。

哈希表即为普通的哈希映射（HashMap），通过缓存数据的键映射到其在双向链表中的位置。

这样以来，我们首先使用哈希表进行定位，找出缓存项在双向链表中的位置，随后将其移动到双向链表的头部，即可在 O(1)O(1)O(1) 的时间内完成 get 或者 put 操作。具体的方法如下：

对于 get 操作，首先判断 key 是否存在：

如果 key 不存在，则返回 −1-1−1；

如果 key 存在，则 key 对应的节点是最近被使用的节点。通过哈希表定位到该节点在双向链表中的位置，并将其移动到双向链表的头部，最后返回该节点的值。

对于 put 操作，首先判断 key 是否存在：

如果 key 不存在，使用 key 和 value 创建一个新的节点，在双向链表的头部添加该节点，并将 key 和该节点添加进哈希表中。然后判断双向链表的节点数是否超出容量，如果超出容量，则删除双向链表的尾部节点，并删除哈希表中对应的项；

如果 key 存在，则与 get 操作类似，先通过哈希表定位，再将对应的节点的值更新为 value，并将该节点移到双向链表的头部。

上述各项操作中，访问哈希表的时间复杂度为 O(1)，在双向链表的头部添加节点、在双向链表的尾部删除节点的复杂度也为 O(1)。而将一个节点移到双向链表的头部，可以分成「删除该节点」和「在双向链表的头部添加节点」两步操作，都可以在 )O(1) 时间内完成。

小贴士

在双向链表的实现中，使用一个伪头部（dummy head）和伪尾部（dummy tail）标记界限，这样在添加节点和删除节点的时候就不需要检查相邻的节点是否存在。
