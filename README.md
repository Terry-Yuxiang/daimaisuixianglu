# 代码随想录刷题总结
## 目录
[第一天](#第一天)     [第二天](#第二天)     [第三天](#第三天)     [第四天](#第四天) [第五天](#第五天) [第六天](#第六天) [第七天](#第七天)
[第八天](#第八天)     [第九天](#第九天)     [第十天](#第十天)

## 数组
### 第一天

### 第二天

## 链表
### 第三天
203. Remove Linked List Elements
重点：虚拟头节点(自己做的时候已经想到)
```
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
    public ListNode removeElements(ListNode head, int val) {
        if (head == null)  return head;
        ListNode newHead = new ListNode();
        ListNode cur = newHead;
        cur.next = head;
        while(cur.next != null) {
            if(cur.next.val == val) {
                cur.next = cur.next.next;
            } else {
                cur = cur.next;
            } 
        }
        return newHead.next;
    }
}
```

707. Design Linked List
重点：链表的综合操作(写的时候还出了一点问题，可以用来综合复习链表练手)
```
public class ListNode {
  int val;
  ListNode next;
  ListNode prev;
  ListNode(int x) { val = x; }
}

class MyLinkedList {
  int size;
  // sentinel nodes as pseudo-head and pseudo-tail
  ListNode head, tail;
  public MyLinkedList() {
    size = 0;
    head = new ListNode(0);
    tail = new ListNode(0);
    head.next = tail;
    tail.prev = head;
  }

  /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
  public int get(int index) {
    // if index is invalid
    if (index < 0 || index >= size) return -1;

    // choose the fastest way: to move from the head
    // or to move from the tail
    ListNode curr = head;
    if (index + 1 < size - index)
      for(int i = 0; i < index + 1; ++i) curr = curr.next;
    else {
      curr = tail;
      for(int i = 0; i < size - index; ++i) curr = curr.prev;
    }

    return curr.val;
  }

  /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
  public void addAtHead(int val) {
    ListNode pred = head, succ = head.next;

    ++size;
    ListNode toAdd = new ListNode(val);
    toAdd.prev = pred;
    toAdd.next = succ;
    pred.next = toAdd;
    succ.prev = toAdd;
  }

  /** Append a node of value val to the last element of the linked list. */
  public void addAtTail(int val) {
    ListNode succ = tail, pred = tail.prev;

    ++size;
    ListNode toAdd = new ListNode(val);
    toAdd.prev = pred;
    toAdd.next = succ;
    pred.next = toAdd;
    succ.prev = toAdd;
  }

  /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
  public void addAtIndex(int index, int val) {
    // If index is greater than the length, 
    // the node will not be inserted.
    if (index > size) return;

    // [so weird] If index is negative, 
    // the node will be inserted at the head of the list.
    if (index < 0) index = 0;

    // find predecessor and successor of the node to be added
    ListNode pred, succ;
    if (index < size - index) {
      pred = head;
      for(int i = 0; i < index; ++i) pred = pred.next;
      succ = pred.next;
    }
    else {
      succ = tail;
      for (int i = 0; i < size - index; ++i) succ = succ.prev;
      pred = succ.prev;
    }

    // insertion itself
    ++size;
    ListNode toAdd = new ListNode(val);
    toAdd.prev = pred;
    toAdd.next = succ;
    pred.next = toAdd;
    succ.prev = toAdd;
  }

  /** Delete the index-th node in the linked list, if the index is valid. */
  public void deleteAtIndex(int index) {
    // if the index is invalid, do nothing
    if (index < 0 || index >= size) return;

    // find predecessor and successor of the node to be deleted
    ListNode pred, succ;
    if (index < size - index) {
      pred = head;
      for(int i = 0; i < index; ++i) pred = pred.next;
      succ = pred.next.next;
    }
    else {
      succ = tail;
      for (int i = 0; i < size - index - 1; ++i) succ = succ.prev;
      pred = succ.prev.prev;
    }

    // delete pred.next 
    --size;
    pred.next = succ;
    succ.prev = pred;
  }
}
```

206. Reverse Linked List
重点：双指针法/递归法(自己做的时候2分钟用双指针AC，复习的时候注意一下递归法)
```
//双指针法 自己写的
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode aheadNode = null;
        ListNode cur = head;
        while(cur != null) {
            ListNode temp = cur.next;
            cur.next = aheadNode;
            aheadNode = cur;
            cur = temp;
        }
        return aheadNode;
    }
}
```

```
// 递归，来自于代码随想录
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }

    private ListNode reverse(ListNode prev, ListNode cur) {
        if (cur == null) {
            return prev;
        }
        ListNode temp = null;
        temp = cur.next;// 先保存下一个节点
        cur.next = prev;// 反转
        // 更新prev、cur位置
        // prev = cur;
        // cur = temp;
        return reverse(cur, temp);
    }
}
```

今天的链表题目比较基础，属于是基本功！

### 第四天

总结写在前面：  
1.注意一下24题的递归的写法，以及19题的快慢指针的思路。  
2.linkedList的题目，画图十分有用！！！  
3.今天的最后一个题leetcode 142题circle linkedlist是经典的快慢指针，需要一些数学计算。  

[24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)  
自己写的时候用双指针的写法比较轻松的AC，注意一下递归的写法。
```
//双指针
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode first = new ListNode();
        first.next = head;
        ListNode cur = first;
        while(cur.next != null) {
            if(cur.next.next != null) {
                // need to change
                ListNode left = cur.next;
                ListNode right = cur.next.next;
                cur.next = right;
                left.next = right.next;
                right.next = left;
                cur = left;
            } else {
                break;
            }
        }
        return first.next;
    }
}
```
```
//递归
// 递归版本
class Solution {
    public ListNode swapPairs(ListNode head) {
        // base case 退出提交
        if(head == null || head.next == null) return head;
        // 获取当前节点的下一个节点
        ListNode next = head.next;
        // 进行递归
        ListNode newNode = swapPairs(next.next);
        // 这里进行交换
        next.next = head;
        head.next = newNode;

        return next;
    }
} 
```
[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
很经典的快慢指针，一开始没有想到，用了ArrayList超过memory limit了。
```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode ahead = new ListNode();
        ahead.next = head;
        ListNode cur = ahead;
        ListNode fast = head;
        for(int i = 0; i < n - 1; i ++) {
            fast = fast.next;
        }
        while(fast != null && fast.next != null) {
            cur = cur.next;
            fast = fast.next;
        }
        cur.next = cur.next.next;
        return ahead.next;
    }
}
```

[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)  
比较简单，只需要先求出两个数组的长度，长的先动就可以。
```
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int nA = 0;
        int nB = 0;
        ListNode curA = headA;
        while(curA != null) {
            curA = curA.next;
            nA++;
        }
        ListNode curB = headB;
        while(curB != null) {
            curB = curB.next;
            nB++;
        }
        curA = headA;
        curB = headB;
        for(int i = 0; i < Math.abs(nB - nA); i++) {
            if(nB >= nA) {
                curB = curB.next;
            } else {
                curA = curA.next;
            }
        }
        while(curA != null) {
            if(curA == curB) {
                return curA;
            }
            curA = curA.next;
            curB = curB.next;
        }
        return null;
    }
}
```
[142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)  
以前做过，有点忘记了，很经典的快慢指针！  
carl哥的答案参考：https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html
```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
                ListNode index1 = fast;
                ListNode index2 = head;
                // 两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```

## 哈希表
### 第一天
242. Valid Anagram

349. Intersection of Two Arrays

202. Happy Number

1. Two Sum

## 字符串
### 第八天
344. Reverse String  
在这个题目比较简单，在这里写一种位运算的swap方法
```
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while (l < r) {
            s[l] ^= s[r];  //构造 a ^ b 的结果，并放在 a 中
            s[r] ^= s[l];  //将 a ^ b 这一结果再 ^ b ，存入b中，此时 b = a, a = a ^ b
            s[l] ^= s[r];  //a ^ b 的结果再 ^ a ，存入 a 中，此时 b = a, a = b 完成交换
            l++;
            r--;
        }
    }
}
```
541. Reverse String II  
重点在于如何处理2k。
 ```
 class Solution {
    public String reverseStr(String s, int k) {
        char[] a = s.toCharArray();
        for (int start = 0; start < a.length; start += 2 * k) {
            int i = start, j = Math.min(start + k - 1, a.length - 1);
            while (i < j) {
                char tmp = a[i];
                a[i++] = a[j];
                a[j--] = tmp;
            }
        }
        return new String(a);
    }
}
 ```
剑指 Offer 05. 替换空格. 
申请额外空间的方法很简单，还有一种做法是先扩充数组，再从后向前用双指针替换空格。    
```
class Solution {
    public String replaceSpace(String s) {
        if(s == null || s.length() == 0){
        return s;
    }
    //扩充空间，空格数量2倍
    StringBuilder str = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
        if(s.charAt(i) == ' '){
            str.append("  ");
        }
    }
    //若是没有空格直接返回
    if(str.length() == 0){
        return s;
    }
    //有空格情况 定义两个指针
    int left = s.length() - 1;//左指针：指向原始字符串最后一个位置
    s += str.toString();
    int right = s.length()-1;//右指针：指向扩展字符串的最后一个位置
    char[] chars = s.toCharArray();
    while(left>=0){
        if(chars[left] == ' '){
            chars[right--] = '0';
            chars[right--] = '2';
            chars[right] = '%';
        }else{
            chars[right] = chars[left];
        }
        left--;
        right--;
    }
    return new String(chars);
    }
}
```
151. Reverse Words in a String
这个题如果用Java的内部方法，分割空格再反转就很简单。主要考虑一下如何不适用额外的空间完成。
```
class Solution {
   /**
     * 不使用Java内置方法实现
     * <p>
     * 1.去除首尾以及中间多余空格
     * 2.反转整个字符串
     * 3.反转各个单词
     */
    public String reverseWords(String s) {
        // System.out.println("ReverseWords.reverseWords2() called with: s = [" + s + "]");
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        // System.out.println("ReverseWords.removeSpace() called with: s = [" + s + "]");
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        // System.out.println("ReverseWords.removeSpace returned: sb = [" + sb + "]");
        return sb;
    }

    /**
     * 反转字符串指定区间[start, end]的字符
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        // System.out.println("ReverseWords.reverseString() called with: sb = [" + sb + "], start = [" + start + "], end = [" + end + "]");
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        // System.out.println("ReverseWords.reverseString returned: sb = [" + sb + "]");
    }

    private void reverseEachWord(StringBuilder sb) {
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseString(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
```

剑指 Offer 58 - II. 左旋转字符串
难点也是如何在不申请额外空间的情况下进行左旋字符串
```
//具体步骤为：
//反转区间为前n的子串
//反转区间为n到末尾的子串
//反转整个字符串

class Solution {
    public String reverseLeftWords(String s, int n) {
        int len=s.length();
        StringBuilder sb=new StringBuilder(s);
        reverseString(sb,0,n-1);
        reverseString(sb,n,len-1);
        return sb.reverse().toString();
    }
     public void reverseString(StringBuilder sb, int start, int end) {
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
            }
        }
}
```

### 第九天   
今天的重点是KMP算法，并且回顾一下双指针。  
28. Find the Index of the First Occurrence in a String
```
// KMP算法，构造next数组
class Solution {

    private int[] next;

    private void contructNext(String needle) {
        int n = needle.length();
        next = new int[n];
        int j = -1;
        next[0] = j;
        for(int i = 1; i < n; i++) {
            while(j >= 0 && needle.charAt(i) != needle.charAt(j + 1)) {
                j = next[j];
            }
            if(needle.charAt(i) == needle.charAt(j + 1)) {
                j++;
            }
            next[i] = j;
        }
    }

    public int strStr(String haystack, String needle) {
        contructNext(needle);
        int j = -1;
        int n = haystack.length();
        for(int i = 0; i < n; i++) {
            while(j >= 0 && needle.charAt(j + 1) != haystack.charAt(i)) {
                j = next[j];
            }
            if(needle.charAt(j + 1) == haystack.charAt(i)) {
                j++;
            }
            if(j == needle.length() - 1) {
                return i - j;
            }
        }
        return -1;
    }
}
```
459. Repeated Substring Pattern.  
这个题目有两种做法，一种是掐头去尾，把两个相同的字符串s加起来，如果掐头去尾后仍然有s，则有repeat。
第二种则是经典的KMP算法。最长相同前后缀的不重合部分则是最小重复子串。
```
// KMP算法的实现
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        if (s.equals("")) return false;

        int len = s.length();
        // 原串加个空格(哨兵)，使下标从1开始，这样j从0开始，也不用初始化了
        s = " " + s;
        char[] chars = s.toCharArray();
        int[] next = new int[len + 1];

        // 构造 next 数组过程，j从0开始(空格)，i从2开始
        for (int i = 2, j = 0; i <= len; i++) {
            // 匹配不成功，j回到前一位置 next 数组所对应的值
            while (j > 0 && chars[i] != chars[j + 1]) j = next[j];
            // 匹配成功，j往后移
            if (chars[i] == chars[j + 1]) j++;
            // 更新 next 数组的值
            next[i] = j;
        }

        // 最后判断是否是重复的子字符串，这里 next[len] 即代表next数组末尾的值
        if (next[len] > 0 && len % (len - next[len]) == 0) {
            return true;
        }
        return false;
    }
}
```
