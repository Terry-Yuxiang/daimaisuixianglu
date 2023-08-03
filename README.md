# 代码随想录刷题总结
## 目录
[第一天](#第一天)     [第二天](#第二天)     [第三天](#第三天)     [第四天](#第四天) [第五天](#第五天) [第六天](#第六天) [第七天](#第七天)
[第八天](#第八天)     [第九天](#第九天)     [第十天](#第十天)     [第十一天](#第十一天)    [第十二天](#第十二天)    [第十三天](#第十三天)
[第十四天](#第十四天)    [第十五天](#第十五天) [第十六天](#第十六天) [第十七天](#第十七天)    [第十八天](#第十八天)
[第十九天](#第十九天)    [第二十天](#第二十天)      [第二十一天](#第二十一天)	[第二十二天](#第二十二天)	[第二十三天](#第二十三天)
[第二十四天](#第二十四天) 	[第二十五天](#第二十五天) [第二十六天](#第二十六天)	[第二十七天](#第二十七天)	[第二十八天](#第二十八天)
[第二十九天](#第二十九天)	[第三十天](#第三十天)	[第三十一天](#第三十一天)	[第三十二天](#第三十二天)	[第三十三天](#第三十三天)
[第三十四天](#第三十四天)	[第三十五天](#第三十五天)	[第三十六天](#第三十六天)	[第三十七天](#第三十七天)	[第三十八天](#第三十八天)
[第三十九天](#第三十九天)	[第四十天](#第四十天)	[第四十一天](#第四十一天)	[第四十二天](#第四十二天)	[第四十三天](#第四十三天)
[第四十四天](#第四十四天)	[第四十五天](#第四十五天)	[第四十六天](#第四十六天)	[第四十七天](#第四十七天)	[第四十八天](#第四十八天)
[第四十九天](#第四十九天)	[第五十天](#第五十天)	[第五十一天](#第五十一天)	[第五十二天](#第五十二天)	[第五十二天](#第五十三天)
[第五十三天](#第五十三天)	[第五十四天](#第五十四天)	[第五十五天](#第五十五天)	[第五十六天](#第五十六天)	[第五十七天](#第五十七天)
[第五十八天](#第五十八天)	[第五十九天](#第五十九天)
## 数组
### 第一天
[704. Binary Search](https://leetcode.com/problems/binary-search/description/)  
二分查找，很基础的题。需要注意的是区间问题。到底是闭区间，还是半开半闭区间
```
// 开区间
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length == 0) {
            return - 1;
        }
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < target) {
                left = mid + 1;
            } else if(nums[mid] > target){
                right = mid - 1;
            } else {
                return mid;
            }
        }
        return -1;
        
    }
}
// 半闭半开
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length == 0) {
            return - 1;
        }
        int left = 0;
        int right = nums.length;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < target) {
                left = mid + 1;
            } else if(nums[mid] > target){
                right = mid;
            } else {
                return mid;
            }
        }
        return -1;
        
    }
}
```
[27. Remove Element](https://leetcode.com/problems/remove-element/description/)   
这个题是双指针法的典型实现，其实双指针在某种程度上来说跟sliding window也很像。
```
class Solution {
    public int removeElement(int[] nums, int val) {
        int n = nums.length;
        int left= 0;
        int right = n - 1;
        int ans = 0;
        while(left <= right) {
            while(left <= right && nums[right] == val) {
                right--;
                ans++;
            }
            if(left <= right && nums[left] == val) {
                nums[left] = nums[right];
                nums[right] = val;
                right--;
                ans++;
            }
            left++;
        }
        return n - ans;
    }
}
```

### 第二天
[977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)  
双指针的题目。用二分法找到最接近0的值是logn，这样做下来是n+logn，如果用for循环找就是2n了。如果是平方再排序那就是n+nlogn，比用双指针要慢很多。
```
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int left = 0;
        int right = n;
        int mid = -1;
        while(left < right) {
            mid = left + (right - left) / 2;
            if(nums[mid] > 0) {
                right = mid;
            } else if (nums[mid] < 0){
                left = mid + 1;
            } else {
                right = mid;
                left = mid - 1;
                break;
            }
        }
        left = right - 1;

        
        int[] ans = new int[n];
        int i = 0;
        while(left >= 0 || right < n) {
            if(left >= 0 && right < n) {
                if(Math.abs(nums[left]) <= Math.abs(nums[right])) {
                    ans[i] = nums[left] * nums[left];
                    left--;
                } else {
                    ans[i] = nums[right] * nums[right];
                    right++;
                }
            } else if(left >= 0){
                ans[i] = nums[left] * nums[left];
                left--;
            } else {
                ans[i] = nums[right] * nums[right];
                right++;
            }
            i++;
        }
        return ans;
    }
}
```

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)   
sliding window的题目，跟双指针有很大的关系。
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int ans = Integer.MAX_VALUE;
        int left = 0;
        int right = 0;
        while(right < nums.length && left <= right) {
            target -= nums[right];
            while(target <= 0) {
                ans = Math.min(ans, right - left + 1);
                target += nums[left];
                left++;
            }
            right++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```

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
[344. Reverse String](https://leetcode.com/problems/reverse-string/)   
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
[541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/description/)  
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
[剑指 Offer 05. 替换空格.](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/) 
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
[151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)   
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
[28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)   


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
[459. Repeated Substring Pattern.](https://leetcode.com/problems/repeated-substring-pattern/)    
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

## 栈与队列
### 第十天  
今天主要是熟悉一下栈和队列的基本数据结构，两个题目都很基础  

[232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

[225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)


### 第十一天
20. Valid Parentheses.  
经典的栈的使用
```
class Solution {
    public boolean isValid(String s) {
        Deque<Character> deque = new LinkedList<>();
        char ch;
        for (int i = 0; i < s.length(); i++) {
            ch = s.charAt(i);
            //碰到左括号，就把相应的右括号入栈
            if (ch == '(') {
                deque.push(')');
            }else if (ch == '{') {
                deque.push('}');
            }else if (ch == '[') {
                deque.push(']');
            } else if (deque.isEmpty() || deque.peek() != ch) {
                return false;
            }else {//如果是右括号判断是否和栈顶元素匹配
                deque.pop();
            }
        }
        //最后判断栈中元素是否匹配
        return deque.isEmpty();
    }
}
```
1047. Remove All Adjacent Duplicates In String.  
也是一个比较经典的栈的题目，可以用栈的特性来进行消除。
这里可以用字符串直接作为栈，这样可以省去用把栈转换为字符串的时间。
```
class Solution {
    public String removeDuplicates(String s) {
        // 将 res 当做栈
        // 也可以用 StringBuilder 来修改字符串，速度更快
        // StringBuilder res = new StringBuilder();
        StringBuffer res = new StringBuffer();
        // top为 res 的长度
        int top = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            // 当 top > 0,即栈中有字符时，当前字符如果和栈中字符相等，弹出栈顶字符，同时 top--
            if (top >= 0 && res.charAt(top) == c) {
                res.deleteCharAt(top);
                top--;
            // 否则，将该字符 入栈，同时top++
            } else {
                res.append(c);
                top++;
            }
        }
        return res.toString();
    }
}
```
150. Evaluate Reverse Polish Notation
同样是比较经典的栈的应用
```
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<String> s = new Stack<>();
        int ans = 0;
        for(int i = 0; i < tokens.length; i++) {
            if(tokens[i].equals("+")) {
                int b = Integer.parseInt(s.pop());
                int a = Integer.parseInt(s.pop());
                s.push(Integer.toString(a + b));
            }
            else if(tokens[i].equals("-")) {
                int b = Integer.parseInt(s.pop());
                int a = Integer.parseInt(s.pop());
                s.push(Integer.toString(a - b));
            }
            else if(tokens[i].equals("*")) {
                int b = Integer.parseInt(s.pop());
                int a = Integer.parseInt(s.pop());
                s.push(Integer.toString(a * b));
            }
            else if(tokens[i].equals("/")) {
                int b = Integer.parseInt(s.pop());
                int a = Integer.parseInt(s.pop());
                s.push(Integer.toString(a / b));
            }
            else {
                s.push(tokens[i]);
            }
        }
        return Integer.parseInt(s.pop());
    }
}
```

### 第十二天
休息日！！！


### 第十三天
239. Sliding Window Maximum
今天的题目难度比较大，使用的单调队列。有两种写法，其中第一种用下标的写法更好理解一些。
```
//解法二
//利用双端队列手动实现单调队列
/**
 * 用一个单调队列来存储对应的下标，每当窗口滑动的时候，直接取队列的头部指针对应的值放入结果集即可
 * 单调队列类似 （tail -->） 3 --> 2 --> 1 --> 0 (--> head) (右边为头结点，元素存的是下标)
 */
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        ArrayDeque<Integer> deque = new ArrayDeque<>();
        int n = nums.length;
        int[] res = new int[n - k + 1];
        int idx = 0;
        for(int i = 0; i < n; i++) {
            // 根据题意，i为nums下标，是要在[i - k + 1, i] 中选到最大值，只需要保证两点
            // 1.队列头结点需要在[i - k + 1, i]范围内，不符合则要弹出
            while(!deque.isEmpty() && deque.peek() < i - k + 1){
                deque.poll();
            }
            // 2.既然是单调，就要保证每次放进去的数字要比末尾的都大，否则也弹出
            while(!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }

            deque.offer(i);

            // 因为单调，当i增长到符合第一个k范围的时候，每滑动一步都将队列头节点放入结果就行了
            if(i >= k - 1){
                res[idx++] = nums[deque.peek()];
            }
        }
        return res;
    }
}

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        ArrayDeque<Integer> deque = new ArrayDeque<>();
        int n = nums.length;
        int[] res = new int[n - k + 1];
        for(int i = 0; i < n; i++) {    
            if(i >= k && !deque.isEmpty() && nums[i - k] == deque.getFirst()) {
                deque.poll();
            }

            while(!deque.isEmpty() && deque.getLast() < nums[i]) {
                deque.removeLast();
            }

            deque.offer(nums[i]);

            if(i >= k - 1) {
                res[i - k + 1] = deque.getFirst();
            }
        }
        return res;
    }
}
```
347. Top K Frequent Elements
这个题目是heap的题目，用到PriorityQueue来做，很经典的heap题目。
大顶堆和小顶堆都可以完成这个题目。
```
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> hashMap = new HashMap<>();
        Queue<Integer> maxHeap = new PriorityQueue<Integer>(
            (o1, o2) -> hashMap.get(o2) - hashMap.get(o1)
        );
        for(int num: nums) {
            hashMap.put(num, hashMap.getOrDefault(num, 0) + 1);
        }
        int[] ans = new int[k];
        for(int num: hashMap.keySet()) {
            maxHeap.add(num);
        }
        for(int i = 0; i < k; i++) {
            ans[i] = maxHeap.poll();
        }
        return ans;
        
    }
}
```

## 二叉树
### 第十四天
今天的内容主要是二叉树的基础知识的回顾  
DFS:前中后序遍历  
BFS:层序遍历  
[144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)
```
//递归的写法
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<Integer>();
        traverse(ans, root);
        return ans;
    }

    public void traverse(List<Integer> ans, TreeNode cur) {
        if(cur == null) return;
        ans.add(cur.val);
        traverse(ans, cur.left);
        traverse(ans, cur.right);
    }
}

//迭代的写法
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<Integer>();
        if(root == null) {
            return ans;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.add(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if(cur != null) {
                ans.add(cur.val);
            }
            if(cur.right != null) stack.push(cur.right);
            if(cur.left != null) stack.push(cur.left);
        }
        return ans;
    }

}
```
[145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)  
```
//递归的写法
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        traverse(ans, root);
        return ans;
    }

    public void traverse(List<Integer> ans, TreeNode cur) {
        if(cur == null) return;
        traverse(ans, cur.left);
        traverse(ans, cur.right);
        ans.add(cur.val);
    }
}

//迭代的写法
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if(root == null) {
            return ans;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            ans.add(cur.val);
            if(cur.left != null) stack.push(cur.left);
            if(cur.right != null) stack.push(cur.right);
        }
        Collections.reverse(ans);
        return ans;
    }
}
```
[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)  
这种的迭代写法变换顺序可以变成前中后序遍历的统一的写法
```
//递归的写法
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        traverse(ans, root);
        return ans;
    }

    public void traverse(List<Integer> ans, TreeNode cur) {
        if(cur == null) return;
        traverse(ans, cur.left);
        ans.add(cur.val);
        traverse(ans, cur.right);
    }
}

//迭代的写法
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if(root == null) return ans;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if(cur != null) {
                if(cur.right != null) stack.push(cur.right);
                stack.push(cur);
                stack.push(null);
                if(cur.left != null) stack.push(cur.left); 
            } else {
                ans.add(stack.pop().val);
            }
        }
        return ans;
    }
}
```

### 第十五天
[102.Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
```
// 102.二叉树的层序遍历
class Solution {
    public List<List<Integer>> resList = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        //checkFun01(root,0);
        checkFun02(root);

        return resList;
    }

    //DFS--递归方式
    public void checkFun01(TreeNode node, Integer deep) {
        if (node == null) return;
        deep++;

        if (resList.size() < deep) {
            //当层级增加时，list的Item也增加，利用list的索引值进行层级界定
            List<Integer> item = new ArrayList<Integer>();
            resList.add(item);
        }
        resList.get(deep - 1).add(node.val);

        checkFun01(node.left, deep);
        checkFun01(node.right, deep);
    }

    //BFS--迭代方式--借助队列
    public void checkFun02(TreeNode node) {
        if (node == null) return;
        Queue<TreeNode> que = new LinkedList<TreeNode>();
        que.offer(node);

        while (!que.isEmpty()) {
            List<Integer> itemList = new ArrayList<Integer>();
            int len = que.size();

            while (len > 0) {
                TreeNode tmpNode = que.poll();
                itemList.add(tmpNode.val);

                if (tmpNode.left != null) que.offer(tmpNode.left);
                if (tmpNode.right != null) que.offer(tmpNode.right);
                len--;
            }

            resList.add(itemList);
        }

    }
}

```
101. Symmetric Tree
```
//回溯
public boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

public boolean isMirror(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return (t1.val == t2.val)
        && isMirror(t1.right, t2.left)
        && isMirror(t1.left, t2.right);
}
```

### 第十六天 
[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/v)  
基础的树求depth的题目，需要牢牢掌握。
```
class Solution {
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```
[111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)  
这个题目求最小值，注意不是简单的把球depth的max改成min就可以的。因为，当一个node只有左或者右的时候，这个高度应该是取有child node的高度。
```
class Solution {
    public int minDepth(TreeNode root) {
        if(root != null) {
            int rightDepth = minDepth(root.right);
            int leftDepth = minDepth(root.left);
            if(rightDepth != 0 && leftDepth != 0) {
                return Math.min(rightDepth, leftDepth) + 1;
            } else {
                return Math.max(rightDepth, leftDepth) + 1;
            }
        }
        return 0;
    }
}

```

[222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)  
这个题是求nodes的个数，如果只是简单用迭代法的话，用O(n)的时间跟完全二叉树其实没有关系。  
为了利用到完全二叉树这个性质，其实可以使用binary search的方法。由于递归法比较简单，下面只贴出binary search的leetcode解法。
```
class Solution {
  // Return tree depth in O(d) time.
  public int computeDepth(TreeNode node) {
    int d = 0;
    while (node.left != null) {
      node = node.left;
      ++d;
    }
    return d;
  }

  // Last level nodes are enumerated from 0 to 2**d - 1 (left -> right).
  // Return True if last level node idx exists. 
  // Binary search with O(d) complexity.
  public boolean exists(int idx, int d, TreeNode node) {
    int left = 0, right = (int)Math.pow(2, d) - 1;
    int pivot;
    for(int i = 0; i < d; ++i) {
      pivot = left + (right - left) / 2;
      if (idx <= pivot) {
        node = node.left;
        right = pivot;
      }
      else {
        node = node.right;
        left = pivot + 1;
      }
    }
    return node != null;
  }

  public int countNodes(TreeNode root) {
    // if the tree is empty
    if (root == null) return 0;

    int d = computeDepth(root);
    // if the tree contains 1 node
    if (d == 0) return 1;

    // Last level nodes are enumerated from 0 to 2**d - 1 (left -> right).
    // Perform binary search to check how many nodes exist.
    int left = 1, right = (int)Math.pow(2, d) - 1;
    int pivot;
    while (left <= right) {
      pivot = left + (right - left) / 2;
      if (exists(pivot, d, root)) left = pivot + 1;
      else right = pivot - 1;
    }

    // The tree contains 2**d - 1 nodes on the first (d - 1) levels
    // and left nodes on the last level.
    return (int)Math.pow(2, d) - 1 + left;
  }
}
```

### 第十七天
[110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)
```
class Solution {
       /**
     * 递归法
     */
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) != -1;
    }

    private int getHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = getHeight(root.left);
        if (leftHeight == -1) {
            return -1;
        }
        int rightHeight = getHeight(root.right);
        if (rightHeight == -1) {
            return -1;
        }
        // 左右子树高度差大于1，return -1表示已经不是平衡树了
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        }
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```
[257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)
```
class Solution {

    private List<String> ans = new ArrayList<String>();

    public List<String> binaryTreePaths(TreeNode root) {
        DFS("", root);
        return this.ans;
    }

    private void DFS(String s, TreeNode cur) {
        if(cur != null) {
            s = (s.equals("") ? cur.val + "" : s + "->" + cur.val);
            DFS(s, cur.left);
            DFS(s, cur.right);
            if(cur.left == null && cur.right == null) {
                this.ans.add(s);
            }
        }
        return;
    }
}
```

[404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)
```
class Solution {

    private int ans = 0;

    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) {
            return 0;
        }
        dfs(root, false, ans);
        return ans;
    }

    public void dfs(TreeNode cur, boolean isLeft, int ans) {
        if(cur != null) {
            if(cur.left != null) dfs(cur.left, true, ans);
            if(cur.right != null) dfs(cur.right, false, ans);
            if(isLeft && cur.left == null && cur.right == null) {
                this.ans += cur.val;
            }
        }
        return;
    }
}
```

### 第十八天
[513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/description/)
自己很容易想出来了迭代的写法，递归参考了一下答案！递归时直接更新每当deep比较大的时候的值就行，因为是大于，所以更新的是最底层leftmost的值。
```
//递归法
class Solution {

    private int depth = -1;
    private int result = 0;

    public int findBottomLeftValue(TreeNode root) {
        bfs(root, 0);
        return this.result;
    }

    private void bfs(TreeNode cur, int deep) {
        if(cur == null) return;
        if(cur.left == null && cur.right == null) {
            if(deep > this.depth) {
                this.result = cur.val;
                this.depth = deep;
            }
        }
        bfs(cur.left, deep + 1);
        bfs(cur.right, deep + 1);
    }
}

//迭代法
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        if(root == null) {
            return 0;
        }
        Queue<TreeNode> que = new LinkedList<>();
        que.add(root);
        int ans = 0;
        while(!que.isEmpty()) {
            int size = que.size();
            ans = que.peek().val;
            for(int i = 0; i < size; i++) {
                TreeNode cur = que.poll();
                if(cur.left != null)    que.add(cur.left);
                if(cur.right != null)   que.add(cur.right);
            }
        }
        return ans;
    }
}
```
[112. Path Sum](https://leetcode.com/problems/path-sum/description/)
自己写的时候用了bfs自增的方式，实际上字节用targetSum自减就可以
```
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        return dfs(root, targetSum, 0);
    }

    private boolean dfs(TreeNode cur, int targetSum, int sum) {
        if(cur != null) {
            sum += cur.val;
            if(cur.left == null && cur.right == null && targetSum == sum) {
                return true;
            }
            return dfs(cur.left, targetSum, sum) || dfs(cur.right, targetSum, sum);
        } else {
            return false;
        }
    }
}

// 代码随想录答案
// lc112 简洁方法
class solution {
    public boolean haspathsum(treenode root, int targetsum) {

        if (root == null) return false; // 为空退出

        // 叶子节点判断是否符合
        if (root.left == null && root.right == null) return root.val == targetsum;

        // 求两侧分支的路径和
        return haspathsum(root.left, targetsum - root.val) || haspathsum(root.right, targetsum - root.val);
    }
}
```

[113. Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)
```
class Solution {

    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        dfs(root, targetSum, new ArrayList<Integer>());
        return ans;
    }

    private void dfs(TreeNode cur, int targetSum, ArrayList<Integer> array) {
        if(cur == null) return;
        
        array.add(cur.val);
        targetSum -= cur.val;
        if(cur.left == null && cur.right == null && targetSum == 0) {
            this.ans.add(new ArrayList<Integer>(array));
        }
        
        dfs(cur.left, targetSum, array);
        dfs(cur.right, targetSum, array);
        array.remove(array.size() - 1);

        return;
    }
}
```
[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)  
注意：  
1.前序和中序可以确定唯一的二叉树。 
2.后序和中序可以确定唯一的二叉树。  
3.前序和后序无法确定唯一的二叉树，因为无法确定分割点的位置，无法确定左右子树。  
这个类题目的重点在于，根据前序（后序）序列，找到中Node，来分割中序。再由中序和前序（后序）的大小相同，来分割前序（后序）。注意分割时，用了开区间还是闭区间。
```
class Solution {

    private HashMap<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
      int n = inorder.length;
      for(int i = 0; i < n; i++) {
        map.put(inorder[i], i);
      }
      return build(inorder, 0, n - 1, postorder, 0, n - 1);
    }

    private TreeNode build(int[] inorder, int inLeft, int inRight, int[] postorder, int postLeft, int postRight) {
      if(inLeft > inRight || postLeft > postRight) {
        return null;
      }
      int midLoc = map.get(postorder[postRight]);
      TreeNode root = new TreeNode(inorder[midLoc]);
      int leftLen = midLoc - inLeft;
      int rightLen = inRight - midLoc;
      root.left = build(inorder, inLeft, midLoc - 1, postorder, postLeft, postLeft + leftLen - 1);
      root.right = build(inorder, midLoc + 1, inRight, postorder, postRight - rightLen, postRight - 1);
      return root;
    }
}
```

[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)  
思路与106的思路是一致的
```
class Solution {

    HashMap<Integer, Integer> loc = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = inorder.length;
        for(int i = 0; i < n; i++) {
            this.loc.put(inorder[i], i);
        }
        return build(preorder, 0, n, inorder, 0, n);
    }

    private TreeNode build(int[] preorder, int preLeft, int preRight, int[] inorder, int inLeft, int inRight) {
        if(preLeft >= preRight || inLeft >= inRight) {
            return null;
        }
        int midLoc = this.loc.get(preorder[preLeft]);
        TreeNode root = new TreeNode(inorder[midLoc]);
        root.left = build(preorder, preLeft + 1, preLeft + (midLoc - inLeft + 1) ,inorder, inLeft, midLoc);
        root.right = build(preorder, preLeft + (midLoc - inLeft + 1), preRight, inorder, midLoc + 1, inRight);

        return root;
    }
}
```

### 第十九天
休息

### 第二十天
[654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)  
```
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return findMaxNode(nums, 0, nums.length);
    }

    private TreeNode findMaxNode(int[] nums, int left, int right) {
        if(left == right) {
            return null;
        }
        int loc = -1;
        int max = Integer.MIN_VALUE;
        for(int i = left; i < right; i++) {
            if(nums[i] > max) {
                loc = i;
                max = nums[i];
            }
        }
        TreeNode root = new TreeNode(max);
        root.left = findMaxNode(nums, left, loc);
        root.right = findMaxNode(nums, loc + 1, right);

        return root;
    }
}
```
[617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)  
下面贴出的是leetcode的答案，自己写出来了，但是判断写的比较麻烦，实际上可以写的很简单。
```
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null)
            return t2;
        if (t2 == null)
            return t1;
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
```

[700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)
因为比较简单，下面只写出回溯做法。

```
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null || root.val == val) {
            return root;
        }
        if(val > root.val) {
            return searchBST(root.right, val);
        } else {
            return searchBST(root.left, val);
        }
    }
}
```

[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)


```
class Solution {
    public boolean validate(TreeNode root, Integer low, Integer high) {
        // Empty trees are valid BSTs.
        if (root == null) {
            return true;
        }
        // The current node's value must be between low and high.
        if ((low != null && root.val <= low) || (high != null && root.val >= high)) {
            return false;
        }
        // The left and right subtree must also be valid.
        return validate(root.right, root.val, high) && validate(root.left, low, root.val);
    }

    public boolean isValidBST(TreeNode root) {
        return validate(root, null, null);
    }
}
```


### 第二十一天

[530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)
这个题可以利用binary search tree的特性，用DFS的中序遍历来算差值（或者构建有序的list）
```
class Solution {
    int minDifference = Integer.MAX_VALUE;
    // Initially, it will be null.
    TreeNode prevNode;

    void inorderTraversal(TreeNode node) {
        if (node == null) {
            return;
        }

        inorderTraversal(node.left);
        // Find the difference with the previous value if it is there.
        if (prevNode != null) {
            minDifference = Math.min(minDifference, node.val - prevNode.val);
        }
        prevNode = node;
        inorderTraversal(node.right);
    }

    int getMinimumDifference(TreeNode root) {
        inorderTraversal(root);
        return minDifference;
    }
};
```

[501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)
这个题不难，主要在于如何计数，以及涉及到用流转换map与list。
```

class Solution {
	public int[] findMode(TreeNode root) {
		Map<Integer, Integer> map = new HashMap<>();
		List<Integer> list = new ArrayList<>();
		if (root == null) return list.stream().mapToInt(Integer::intValue).toArray();
		// 获得频率 Map
		searchBST(root, map);
		List<Map.Entry<Integer, Integer>> mapList = map.entrySet().stream()
				.sorted((c1, c2) -> c2.getValue().compareTo(c1.getValue()))
				.collect(Collectors.toList());
		list.add(mapList.get(0).getKey());
		// 把频率最高的加入 list
		for (int i = 1; i < mapList.size(); i++) {
			if (mapList.get(i).getValue() == mapList.get(i - 1).getValue()) {
				list.add(mapList.get(i).getKey());
			} else {
				break;
			}
		}
		return list.stream().mapToInt(Integer::intValue).toArray();
	}

	void searchBST(TreeNode curr, Map<Integer, Integer> map) {
		if (curr == null) return;
		map.put(curr.val, map.getOrDefault(curr.val, 0) + 1);
		searchBST(curr.left, map);
		searchBST(curr.right, map);
	}

}
```

[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
这个题目的关键在于，如何找到高度最小的公共节点，用left和right以及self计数的方式可以解决这个问题。在计数的时候，因为采用right和left共同计数，这样求道的一定是最近的节点，因为再往上追溯，一定只是这个节点的一边。
```
//leetcode官方的计数解法
//实际上不需要这么复杂，可以直接用left和right的null值来计数，这样消耗的空间比较少。
//参考代码随想录的答案
class Solution {

    private TreeNode ans;

    public Solution() {
        // Variable to store LCA node.
        this.ans = null;
    }

    private boolean recurseTree(TreeNode currentNode, TreeNode p, TreeNode q) {

        // If reached the end of a branch, return false.
        if (currentNode == null) {
            return false;
        }

        // Left Recursion. If left recursion returns true, set left = 1 else 0
        int left = this.recurseTree(currentNode.left, p, q) ? 1 : 0;

        // Right Recursion
        int right = this.recurseTree(currentNode.right, p, q) ? 1 : 0;

        // If the current node is one of p or q
        int mid = (currentNode == p || currentNode == q) ? 1 : 0;


        // If any two of the flags left, right or mid become True
        if (mid + left + right >= 2) {
            this.ans = currentNode;
        }

        // Return true if any one of the three bool values is True.
        return (mid + left + right > 0);
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Traverse the tree
        this.recurseTree(root, p, q);
        return this.ans;
    }
}

//代码随想录解法
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) { // 递归结束条件
            return root;
        }

        // 后序遍历
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if(left == null && right == null) { // 若未找到节点 p 或 q
            return null;
        }else if(left == null && right != null) { // 若找到一个节点
            return right;
        }else if(left != null && right == null) { // 若找到一个节点
            return left;
        }else { // 若找到两个节点
            return root;
        }
    }
}
```

### 第二十二天
[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)   
跟236很像，但是这个是搜索二叉树，可以利用BST的特性来做。用236的方法也是可以的，但是这样时间比较短。
```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) {
            return root;
        }
        int big = Math.max(p.val, q.val);
        int small = Math.min(p.val, q.val);
        if(root.val <= big && root.val >= small) {
            return root;
        }
        if(root.val > big) {
            return lowestCommonAncestor(root.left, p, q);
        } else {
            return lowestCommonAncestor(root.right, p, q);
        }
    }
}
```
[701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)  
这个题其实无需重构，只需要利用BST的性质，寻找对的空节点插入即可。
```
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        TreeNode newRoot = root;
        TreeNode pre = root;
        while (root != null) {
            pre = root;
            if (root.val > val) {
                root = root.left;
            } else if (root.val < val) {
                root = root.right;
            } 
        }
        if (pre.val > val) {
            pre.left = new TreeNode(val);
        } else {
            pre.right = new TreeNode(val);
        }

        return newRoot;
    }
}
```
[450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)  
这个题目还是有一定难度的，特别是递归写法！！！
```
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        root = delete(root,key);
        return root;
    }

    private TreeNode delete(TreeNode root, int key) {
        if (root == null) return null;

        if (root.val > key) {
            root.left = delete(root.left,key);
        } else if (root.val < key) {
            root.right = delete(root.right,key);
        } else {
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            TreeNode tmp = root.right;
            while (tmp.left != null) {
                tmp = tmp.left;
            }
            root.val = tmp.val;
            root.right = delete(root.right,tmp.val);
        }
        return root;
    }
}
```



### 第二十三天
[669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/description/)  
因为这个题再题目中限定，被删除的节点只会有一个相邻的child node，大大简化的难度。用回溯完成，把握好写回溯的三要素即可。
```
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if(root == null) {
            return null;
        }
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        int val = root.val;
        if(val < low || val > high) {
            return root.left == null ? root.right : root.left;
        }
        return root;
    }
}
```

[108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)    
遵循左闭右开原则，一遍过。
```
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        int n = nums.length;
        return constructBST(nums, 0, n);
    }

    private TreeNode constructBST(int[] nums, int begin, int end) {
        //用左闭右开
        if(begin >= end) {
            return null;
        }
        int mid = begin + (end - begin) / 2;
        TreeNode node = new TreeNode(nums[mid]);
        node.left = constructBST(nums, begin, mid);
        node.right = constructBST(nums, mid + 1, end);

        return node;
    }
}
```
[538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)  
遵循右 -> 中 -> 左 遍历，很容易把代码写出来，与递归遍历代码基本一致，只是多了一个求和的步骤。
```
class Solution {

    private int sum = 0;

    // 右 -> 中 -> 左 遍历
    public TreeNode convertBST(TreeNode root) {
        if(root == null) {
            return root;
        }
        root.right = convertBST(root.right);
        
        int val = root.val;
        root.val += sum;
        sum += val;

        root.left = convertBST(root.left);
        return root;
    }
}
```

## 回溯算法
### 第二十四天

[77. Combinations](https://leetcode.com/problems/combinations/description/)  
今天是回溯算法的第一题，一道经典的回溯题！
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        combineHelper(n, k, 1);
        return result;
    }

    /**
     * 每次从集合中选取元素，可选择的范围随着选择的进行而收缩，调整可选择的范围，就是要靠startIndex
     * @param startIndex 用来记录本层递归的中，集合从哪里开始遍历（集合就是[1,...,n] ）。
     */
    private void combineHelper(int n, int k, int startIndex){
        //终止条件
        if (path.size() == k){
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i <= n - (k - path.size()) + 1; i++){
            path.add(i);
            combineHelper(n, k, i + 1);
            path.removeLast();
        }
    }
}
```
### 第二十五天
[216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)  
与昨天的组合很像。
```
class Solution {

    private List<List<Integer>> ans = new ArrayList<>();
    
    private LinkedList<Integer> oneAns = new LinkedList<>();

    public List<List<Integer>> combinationSum3(int k, int n) {
        backTrack(k, n, 1, 0);
        return ans;
    }

    private void backTrack(int k, int n, int begin, int sum) {
        if(oneAns.size() == k) {
            return;
        }

        if(begin == 10) {
            return;
        }

        for(int i = begin; i < 10; i++) {
            sum += i;
            oneAns.add(i);
            backTrack(k, n, i + 1, sum);
            if(oneAns.size() == k && sum == n) {
                ans.add(new ArrayList<>(oneAns));
            }
            sum -= i;
            oneAns.removeLast();
        }

        return;

    }
}
```

[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)  
构建了一个map，剩下的backtrack与前两题相似。
```
class Solution {

    private HashMap<Character, char[]> map = new HashMap<>();

    private List<String> ans = new ArrayList<>();

    private String oneAns = "";

    public List<String> letterCombinations(String digits) {
        map.put('2', new char[]{'a', 'b', 'c'});
        map.put('3', new char[]{'d', 'e', 'f'});
        map.put('4', new char[]{'g', 'h', 'i'});
        map.put('5', new char[]{'j', 'k', 'l'});
        map.put('6', new char[]{'m', 'n', 'o'});
        map.put('7', new char[]{'p', 'q', 'r', 's'});
        map.put('8', new char[]{'t', 'u', 'v'});
        map.put('9', new char[]{'w', 'x', 'y', 'z'});
        backtrack(digits, 0);
        return ans;
    }

    private void backtrack(String digits, int loc) {
        if(loc >= digits.length()) {
            if(oneAns.length() != 0) {
                ans.add(oneAns);
            }
            return;
        }

        for(int i = 0; i < map.get(digits.charAt(loc)).length; i++) {
            oneAns += map.get(digits.charAt(loc))[i];
            backtrack(digits, loc + 1);
            oneAns = oneAns.substring(0, oneAns.length() - 1);
        }
        return;
    }

    
}
```
### 第二十六天
周末休息，复习了一下kmp算法。

### 第二十七天
今天继续是回溯问题。  

[39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)  
因为前几天的leetcode的每日一题刚好也是回溯，做了一道类似的hard题，这个题比较轻松的AC了。下面第一个是我自己写的答案，我没有用for循环，而是利用两次回调来达成的目的，代码随想录给的范例用了for循环，而且先排序来进行剪枝。我虽然判断了剪枝的条件，但是因为没有先对数组进行排序，所以效果并不明显。
```
// 自己写的答案
class Solution {

    private LinkedList<Integer> one = new LinkedList<>();

    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        backtrack(candidates, target, 0);
        return ans;
    }

    private void backtrack(int[] candidates, int target, int index) {
        if(target == 0) {
            ans.add(new ArrayList<>(one));
            return;
        }
        if(index == candidates.length || target < 0) {
            return;
        }


        target -= candidates[index];
        one.add(candidates[index]);
        backtrack(candidates, target, index);

        target += candidates[index];
        one.removeLast();
        backtrack(candidates, target, index + 1);


        return;
    }

}

//根据代码随想录的题解写的
class Solution {

    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtrack(candidates, target, new LinkedList<>(), 0);
        return ans;
    }

    private void backtrack(int[] candidates, int target, LinkedList<Integer> one, int index) {
        if(target == 0) {
            ans.add(new ArrayList<>(one));
            return;
        }
        if(target < 0) {
            return;
        }
        for(int i = index; i < candidates.length; i++) {
            one.add(candidates[i]);
            backtrack(candidates, target - candidates[i], one, i);
            one.removeLast();
        }

        return;
    }

}
```
[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)  
用set方法做会超时！！！！！！   
这个题的主要难点在于怎么去重复！！
```
class Solution {
  LinkedList<Integer> path = new LinkedList<>();
  List<List<Integer>> ans = new ArrayList<>();
  boolean[] used;
  int sum = 0;

  public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    used = new boolean[candidates.length];
    // 加标志数组，用来辅助判断同层节点是否已经遍历
    Arrays.fill(used, false);
    // 为了将重复的数字都放到一起，所以先进行排序
    Arrays.sort(candidates);
    backTracking(candidates, target, 0);
    return ans;
  }

  private void backTracking(int[] candidates, int target, int startIndex) {
    if (sum == target) {
      ans.add(new ArrayList(path));
    }
    for (int i = startIndex; i < candidates.length; i++) {
      if (sum + candidates[i] > target) {
        break;
      }
      // 出现重复节点，同层的第一个节点已经被访问过，所以直接跳过
      if (i > 0 && candidates[i] == candidates[i - 1] && !used[i - 1]) {
        continue;
      }
      used[i] = true;
      sum += candidates[i];
      path.add(candidates[i]);
      // 每个节点仅能选择一次，所以从下一位开始
      backTracking(candidates, target, i + 1);
      used[i] = false;
      sum -= candidates[i];
      path.removeLast();
    }
  }
}
```

[131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)  
这个题是回溯的一个应用了，分割其实就是树的每一种遍历。也是运用for做横向遍历和递归来纵向遍历来做。
```
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<List<String>>();
        dfs(0, result, new ArrayList<String>(), s);
        return result;
    }

    void dfs(int start, List<List<String>> result, List<String> currentList, String s) {
        if (start >= s.length()) result.add(new ArrayList<String>(currentList));
        for (int end = start; end < s.length(); end++) {
            if (isPalindrome(s, start, end)) {
                // add current substring in the currentList
                currentList.add(s.substring(start, end + 1));
                dfs(end + 1, result, currentList, s);
                // backtrack and remove the current substring from currentList
                currentList.remove(currentList.size() - 1);
            }
        }
    }

    boolean isPalindrome(String s, int low, int high) {
        while (low < high) {
            if (s.charAt(low++) != s.charAt(high--)) return false;
        }
        return true;
    }
}
```

### 第二十八天
今天继续做回溯问题的题。   
[93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)  
这个题目跟分割palindorme是很像的，只是判断的边界条件要多一些。
```
class Solution {
    private List<String> ans = new ArrayList<>();

    public List<String> restoreIpAddresses(String s) {
        backtrack(s, 0, "", 3);
        return ans;
    }

    private void backtrack(String s, int start, String ip, int index) {
        int n = s.length();
        if(index == -1 && start != n) {
            return;
        }
        if(start == n) {
            ans.add(ip.substring(0, ip.length() - 1));
            return;
        }
        for(int i = start; i < start + 3; i++) {
            if(i >= n || n - i - 1 < index) {
                return;
            }
            if((n - i - 1 > index * 3)) {
                continue;
            } else {
                String a = s.substring(start, i + 1);
                if(Integer.valueOf(a) > 255 || (a.charAt(0) == '0' && a.length() != 1))  {
                    return;
                }
                backtrack(s, i + 1, ip + a + ".", index - 1);
            }
        }
        return;
    }
}
```

[78. Subsets](https://leetcode.com/problems/subsets/description/)  
这个题是个子集问题，其实就是收集树的每一个node的结果，经过前几天的训练很快速的AC了这个题。
```
class Solution {

    private ArrayList<Integer> one = new ArrayList<Integer>();

    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        backtrack(nums, 0);
        return ans;
    }

    private void backtrack(int[] nums, int index) {
        int n = nums.length;
        ans.add(new ArrayList<>(one));
        for(int i = index; i < n; i++) {
            one.add(nums[i]);
            backtrack(nums, i + 1);
            one.remove(one.size() - 1);
        }
        return;
    }
}
```
[90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)  
这个题和上个题目相似，只是因为不是unique所以需要去重。参考昨天的40题，也是去重复，可以用一个标记数组来解决这个问题。
```
class Solution {

    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        boolean[] b = new boolean[nums.length];
        backtrack(nums, b, 0, new ArrayList<>());
        return ans;
    }

    private void backtrack(int[] nums, boolean[] b, int index, ArrayList<Integer> one) {
        int n = nums.length;
        ans.add(new ArrayList<>(one));
        for(int i = index; i < n; i++) {
            if(i > 0 && nums[i] == nums[i - 1] && !b[i - 1]) {
                continue;
            }
            one.add(nums[i]);
            b[i] = true;
            backtrack(nums, b, i + 1, one);
            b[i] = false;
            one.remove(one.size() - 1);
        }
        return;
    }
}
```

### 第二十九天
[491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)  
这个题和90题很像，主要是去重复的考虑。因为这个没法将数组进行sort排序，所以在每一层用set去重。
```
class Solution {

    private List<Integer> one = new ArrayList<>();
    
    private List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> findSubsequences(int[] nums) {
        backtrack(nums, 0);
        return ans;
    }

    private void backtrack(int[] nums, int index) {
        if(one.size() >= 2) {
            ans.add(new ArrayList<>(one));
        }
        
        HashSet<Integer> set = new HashSet<>();
        for(int i = index; i < nums.length; i++) {
            if(!set.contains(nums[i]) &&
            (one.size() == 0 || (nums[i] >= one.get(one.size() - 1)))) {
                set.add(nums[i]);
                one.add(nums[i]);
                backtrack(nums, i + 1);
                one.remove(one.size() - 1);
            }
        }
        return;
    }
}
```

[46. Permutations](https://leetcode.com/problems/permutations/description/)  
这个题目是全排列问题，与前面题目的区别主要在于，如何在横向遍历的时候不要取到已经取过的重复元素。解决这个办法的最好问题是构建一个数组，用下标是否在子树中访问过来判断。
```
class Solution {

    private LinkedList<Integer> one = new LinkedList<>();

    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        backtrack(nums, new boolean[nums.length], 0);
        return ans;
    }

    private void backtrack(int[] nums, boolean[] ary, int count) {
        if(one.size() == nums.length) {
            ans.add(new ArrayList<>(one));
        }
        if(count == nums.length) {
            return;
        }
        
        for(int i = 0; i < nums.length; i++) {
            if(ary[i] == true) {
                continue;
            }
            ary[i] = true;
            one.add(nums[i]);
            backtrack(nums, ary, count + 1);
            one.removeLast();
            ary[i] = false;
        }
        return;
    }
}
```

[47. Permutations II](https://leetcode.com/problems/permutations-ii/description/)  
这个题跟前几天做的题的思路一样，先sort数组，然后用used数组进行去重。  
这里有一个点需要注意，在去重复的时候used数组 == false和 == true都是可以去重复的，只是去重复在树上的位置不同。  
用 used[i] == false 去重复可以减少很多无谓的遍历，很大程度上提高效率！  
具体参考：https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html#%E6%8B%93%E5%B1%95
```
class Solution {

    private LinkedList<Integer> one = new LinkedList<>();

    private List<List<Integer>> ans = new ArrayList<>();

    private boolean[] ary;

    private boolean[] noRepeat;

    public List<List<Integer>> permuteUnique(int[] nums) {
        ary = new boolean[nums.length];
        noRepeat = new boolean[nums.length];
        Arrays.fill(noRepeat, false);
        Arrays.sort(nums);
        backtrack(nums);
        return ans;
    }

    private void backtrack(int[] nums) {
        if(one.size() == nums.length) {
            ans.add(new ArrayList<>(one));
        }
        for(int i = 0; i < nums.length; i++) {
            if(ary[i] == true) {
                continue;
            }
            if(i > 0 && nums[i] == nums[i - 1] && !noRepeat[i - 1]) {
                continue;
            }

            noRepeat[i] = true;
            ary[i] = true;
            one.add(nums[i]);
            backtrack(nums);
            one.removeLast();
            ary[i] = false;
            noRepeat[i] = false;
            
            
        }
        return;
    }
}
```
### 第三十天
今天的三个题的难度都很大，主要是是对回溯算法的应用
[332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)    
看了答案才做出来，有点抽象不是很理解。需要好好再回顾一下。
```
class Solution {
    private LinkedList<String> res;
    private LinkedList<String> path = new LinkedList<>();

    public List<String> findItinerary(List<List<String>> tickets) {
        Collections.sort(tickets, (a, b) -> a.get(1).compareTo(b.get(1)));
        path.add("JFK");
        boolean[] used = new boolean[tickets.size()];
        backTracking((ArrayList) tickets, used);
        return res;
    }

    public boolean backTracking(ArrayList<List<String>> tickets, boolean[] used) {
        if (path.size() == tickets.size() + 1) {
            res = new LinkedList(path);
            return true;
        }

        for (int i = 0; i < tickets.size(); i++) {
            if (!used[i] && tickets.get(i).get(0).equals(path.getLast())) {
                path.add(tickets.get(i).get(1));
                used[i] = true;

                if (backTracking(tickets, used)) {
                    return true;
                }

                used[i] = false;
                path.removeLast();
            }
        }
        return false;
    }
}

```

[51. N-Queens](https://leetcode.com/problems/n-queens/description/)  
这个题难度在于如果运用回溯法的话，需要理解什么代表树的宽度，什么代表高度，并且用二维数组来进行判断条件。
个人认为比上一道题更好理解一些。
```
class Solution {
    
    private LinkedList<String> one = new LinkedList<>();

    private List<List<String>> ans = new ArrayList<>();

    private boolean[][] used;

    private char[] charArray;

    public List<List<String>> solveNQueens(int n) {
        used = new boolean[n][n];
        charArray = new char[n];
        for(int i = 0; i < n; i++) {
            charArray[i] = '.';
        }
        backtrack(n, 0);
        return ans;
    }

    private void backtrack(int n, int deep) {
        if(deep == n) {
            ans.add(new ArrayList<>(one));
            return;
        }

        for(int i = 0; i < n; i++) {
            if(!isValid(deep, i, n, used)) {
                continue;
            }
            this.charArray[i] = 'Q';
            String s = new String(charArray);
            charArray[i] = '.';
            one.add(s);
            used[deep][i] = true;
            backtrack(n, deep + 1);
            one.removeLast();
            used[deep][i] = false;
        }
    }

    private boolean isValid(int row, int col, int n, boolean[][] chessboard) {
        // 检查列
        for (int i=0; i<row; ++i) { // 相当于剪枝
            if (chessboard[i][col]) {
                return false;
            }
        }

        // 检查45度对角线
        for (int i=row-1, j=col-1; i>=0 && j>=0; i--, j--) {
            if (chessboard[i][j]) {
                return false;
            }
        }

        // 检查135度对角线
        for (int i=row-1, j=col+1; i>=0 && j<=n-1; i--, j++) {
            if (chessboard[i][j]) {
                return false;
            }
        }
        return true;
    }
}
```
[37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)  
这个题我觉得难点主要是判断是否符合条件上，还有就是如何利用递归来遍历1-9这九个数字，如何与两个for循环结合起来。
```
class Solution {
    public void solveSudoku(char[][] board) {
        solveSudokuHelper(board);
    }

    private boolean solveSudokuHelper(char[][] board){
        //「一个for循环遍历棋盘的行，一个for循环遍历棋盘的列，
        // 一行一列确定下来之后，递归遍历这个位置放9个数字的可能性！」
        for (int i = 0; i < 9; i++){ // 遍历行
            for (int j = 0; j < 9; j++){ // 遍历列
                if (board[i][j] != '.'){ // 跳过原始数字
                    continue;
                }
                for (char k = '1'; k <= '9'; k++){ // (i, j) 这个位置放k是否合适
                    if (isValidSudoku(i, j, k, board)){
                        board[i][j] = k;
                        if (solveSudokuHelper(board)){ // 如果找到合适一组立刻返回
                            return true;
                        }
                        board[i][j] = '.';
                    }
                }
                // 9个数都试完了，都不行，那么就返回false
                return false;
                // 因为如果一行一列确定下来了，这里尝试了9个数都不行，说明这个棋盘找不到解决数独问题的解！
                // 那么会直接返回， 「这也就是为什么没有终止条件也不会永远填不满棋盘而无限递归下去！」
            }
        }
        // 遍历完没有返回false，说明找到了合适棋盘位置了
        return true;
    }

    /**
     * 判断棋盘是否合法有如下三个维度:
     *     同行是否重复
     *     同列是否重复
     *     9宫格里是否重复
     */
    private boolean isValidSudoku(int row, int col, char val, char[][] board){
        // 同行是否重复
        for (int i = 0; i < 9; i++){
            if (board[row][i] == val){
                return false;
            }
        }
        // 同列是否重复
        for (int j = 0; j < 9; j++){
            if (board[j][col] == val){
                return false;
            }
        }
        // 9宫格里是否重复
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++){
            for (int j = startCol; j < startCol + 3; j++){
                if (board[i][j] == val){
                    return false;
                }
            }
        }
        return true;
    }
}
```
## 贪心问题
### 第三十一天
[455. Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)   
可以分为优先满足小胃口和优先满足大胃口两种做题思路。
```
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int indexG = 0;
        int indexS = 0;
        int ans = 0;
        while(indexG < g.length && indexS < s.length) {
            if (s[indexS] >= g[indexG]) {
                indexG++;
                ans++;
            }
            indexS++;
        }
        return ans;
    }
}
```

[376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)  
```
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length <= 1) {
            return nums.length;
        }
        //当前差值
        int curDiff = 0;
        //上一个差值
        int preDiff = 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            //得到当前差值
            curDiff = nums[i] - nums[i - 1];
            //如果当前差值和上一个差值为一正一负
            //等于0的情况表示初始时的preDiff
            if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
                count++;
                preDiff = curDiff;
            }
        }
        return count;
    }
}
```

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)  
用贪心和动态规划都可以很好的完成这个题目。
```
// 贪心
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 1){
            return nums[0];
        }
        int sum = Integer.MIN_VALUE;
        int count = 0;
        for (int i = 0; i < nums.length; i++){
            count += nums[i];
            sum = Math.max(sum, count); // 取区间累计的最大值（相当于不断确定最大子序终止位置）
            if (count <= 0){
                count = 0; // 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
            }
        }
       return sum;
    }
}

// 动态规划
// DP 方法
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = Integer.MIN_VALUE;
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        ans = dp[0];

        for (int i = 1; i < nums.length; i++){
            dp[i] = Math.max(dp[i-1] + nums[i], nums[i]);
            ans = Math.max(dp[i], ans);
        }

        return ans;
    }
}

```

### 第三十二天
[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)  
这个题是典型的贪心的问题，想要求的最终利益的最大值，可以求的单天利润的大值，即需要把单天利益为正的相加即可。这个题目也可以用动态规划求解。
```
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        for(int i = 1; i < prices.length; i++) {
            if(prices[i] - prices[i - 1] > 0) {
                ans += prices[i] - prices[i - 1];
            }
        }
        return ans;
    }
}
```

[55. Jump Game](https://leetcode.com/problems/jump-game/description/)  

```
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        for(int i = 0; i < nums.length; i++) {
            if(maxReach >= i) {
                maxReach = Math.max(maxReach, nums[i] + i);
                if(maxReach >= nums.length - 1) {
                    return true;
                }  
            }
        }
        return false;
    }
}
```

[45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)   
```
// 自己写的版本
class Solution {
    public int jump(int[] nums) {
        int ans = 0;
        int maxReach = 0;
        for(int i = 0; i < nums.length;) {
            if(maxReach >= nums.length - 1) {
                return ans;
            }
            int oldMax = maxReach;
            while(i <= oldMax) {
                maxReach = Math.max(maxReach, i + nums[i]);
                i++;
            }
            ans++;
        }
        return ans;
    }
}

// 代码随想录提供的两个版本
// 版本一
class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length == 0 || nums.length == 1) {
            return 0;
        }
        //记录跳跃的次数
        int count=0;
        //当前的覆盖最大区域
        int curDistance = 0;
        //最大的覆盖区域
        int maxDistance = 0;
        for (int i = 0; i < nums.length; i++) {
            //在可覆盖区域内更新最大的覆盖区域
            maxDistance = Math.max(maxDistance,i+nums[i]);
            //说明当前一步，再跳一步就到达了末尾
            if (maxDistance>=nums.length-1){
                count++;
                break;
            }
            //走到当前覆盖的最大区域时，更新下一步可达的最大区域
            if (i==curDistance){
                curDistance = maxDistance;
                count++;
            }
        }
        return count;
    }
}

// 版本二
class Solution {
    public int jump(int[] nums) {
        int result = 0;
        // 当前覆盖的最远距离下标
        int end = 0;
        // 下一步覆盖的最远距离下标
        int temp = 0;
        for (int i = 0; i <= end && end < nums.length - 1; ++i) {
            temp = Math.max(temp, i + nums[i]);
            // 可达位置的改变次数就是跳跃次数
            if (i == end) {
                end = temp;
                result++;
            }
        }
        return result;
    }
}

```

### 第三十三天
休息日

### 第三十四天
今天继续是贪心问题。   
[1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)  
```
class Solution {
    public int largestSumAfterKNegations(int[] nums, int K) {
    	// 将数组按照绝对值大小从大到小排序，注意要按照绝对值的大小
	nums = IntStream.of(nums)
		     .boxed()
		     .sorted((o1, o2) -> Math.abs(o2) - Math.abs(o1))
		     .mapToInt(Integer::intValue).toArray();
	int len = nums.length;	    
	for (int i = 0; i < len; i++) {
	    //从前向后遍历，遇到负数将其变为正数，同时K--
	    if (nums[i] < 0 && K > 0) {
	    	nums[i] = -nums[i];
	    	K--;
	    }
	}
	// 如果K还大于0，那么反复转变数值最小的元素，将K用完

	if (K % 2 == 1) nums[len - 1] = -nums[len - 1];
	return Arrays.stream(nums).sum();

    }
}
```
[134. Gas Station](https://leetcode.com/problems/gas-station/)  
这个题的贪心思路不是特别明确，之前做的时候就卡壳了，现在做又卡壳了。下面的答案是局部greedy的，也是传统greedy的思路。还可以用全局贪心做这个题，不过不属于传统贪心的范畴。
```
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int curSum = 0;
        int totalSum = 0;
        int index = 0;
        for (int i = 0; i < gas.length; i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {
                index = (i + 1); 
                curSum = 0;
            }
        }
        if (totalSum < 0) return -1;
        return index;
    }
}
```
[135. Candy](https://leetcode.com/problems/candy/description/)  
这个题的核心思想是，把相邻的最大，拆分成从左到右发糖果，如果右边大就多发一个，否则就发一个糖果。和从右往左发糖果，如果左边大就多发一个，否则就发一个糖果的问题。
```
class Solution {
    public int candy(int[] ratings) {
        int[] candy = new int[ratings.length];
        candy[0] = 1;
        for(int i = 1; i < ratings.length; i++) {
            if(ratings[i] <= ratings[i - 1]) {
                candy[i] = 1;
            } else {
                candy[i] = candy[i - 1] + 1;
            }
        }
        for(int i = ratings.length - 2; i >= 0; i--) {
            if(ratings[i] > ratings[i + 1]) {
                candy[i] = Math.max(candy[i + 1] + 1, candy[i]);
            }
        }
        int ans = 0;
        for(int every : candy) {
            ans += every;
        }
        return ans;
    }
}
```

### 第三十五天
[860. Lemonade Change](https://leetcode.com/problems/lemonade-change/)  
这个题其实思路很简单，优先找大钱就可以了。

[406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)   
这个题跟135题发糖果很像，分两次排序，每一次都找最优的情况。
```
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // 身高从大到小排（身高相同k小的站前面）
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];   // a - b 是升序排列，故在a[0] == b[0]的狀況下，會根據k值升序排列
            return b[0] - a[0];   //b - a 是降序排列，在a[0] != b[0]，的狀況會根據h值降序排列
        });

        LinkedList<int[]> que = new LinkedList<>();

        for (int[] p : people) {
            que.add(p[1],p);   //Linkedlist.add(index, value)，會將value插入到指定index裡。
        }

        return que.toArray(new int[people.length][]);
    }
}
```
[452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)  
之前做过一次，这一次自己AC了，但是遇到很多问题，需要注意为了不造成integer overflow，最好用比大小判断，而不要用相减。
```
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (o1, o2) -> {
            // 注意这里不能用o1 - o2， 因为可能会造成integer overflow
            if (o1[1] == o2[1]) return 0;
            if (o1[1] < o2[1]) return -1;
            return 1;
        });
        int ans = 1;
        int right = points[0][1];
        for(int i = 0; i < points.length; i++) {
            if(right >= points[i][0]) {
                continue;
            }
            ans++;
            right = points[i][1];
        }
        return ans;
    }
}
```

### 第三十六天
[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)  
这个题跟432其实很像，但是其实贪心的思路又不太一样。
为了能够保存最多的时间，想一想，我们应该应该总是把后面overlap的时间去掉，所以队intervals[i][1]进行从小到大的排序。
这里greedy的思路是，保留越早结束的时间，我们就越有可能在后面不overlap。
```
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, Comparator.comparingInt(a -> a[1]));
        int ans = 0;
        int k = Integer.MIN_VALUE;
        
        for (int i = 0; i < intervals.length; i++) {
            int x = intervals[i][0];
            int y = intervals[i][1];
            
            if (x >= k) {
                // Case 1
                k = y;
            } else {
                // Case 2
                ans++;
            }
        }
        
        return ans;
    }
}
```

[763. Partition Labels](https://leetcode.com/problems/partition-labels/description/)  
这个题如何区分字母呢，我们需要可以先遍历一遍，找到每个字母的最远位置，再次遍历的时候，如果这个字母所在字符串的最远位置与这个字母的位置相等，则说明可以这样分割。
```
class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] loc = new int[26];
        for(int i = 0; i < s.length(); i++) {
            loc[s.charAt(i) - 'a'] = i;
        }
        List<Integer> ans = new ArrayList<>();
        int left = 0;
        int right = 0;
        for(int i = 0; i < s.length(); i++) {
            right = Math.max(loc[s.charAt(i) - 'a'], right);
            if(right == i) {
                ans.add(i - left + 1);
                left = i + 1;
            }
        }
        return ans;
    }
}
```

[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)   
我觉得这个题目贪心的思路还是很好想的，其实跟452和435的思路是很像的，都是先sort，然后利用最大化的思想来完成。
```
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        LinkedList<int[]> merged = new LinkedList<>();
        for (int[] interval : intervals) {
            // if the list of merged intervals is empty or if the current
            // interval does not overlap with the previous, simply append it.
            if (merged.isEmpty() || merged.getLast()[1] < interval[0]) {
                merged.add(interval);
            }
            // otherwise, there is overlap, so we merge the current and previous
            // intervals.
            else {
                merged.getLast()[1] = Math.max(merged.getLast()[1], interval[1]);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```
### 第三十七天
[738. Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/description/)     
这个题想到了贪心的思路，但是没想清楚怎么处理类似于数字'989998'这样的问题。看了代码随想录的答案，可以记录一下9的位置，让其后都是9，之后再填充。
```
class Solution {
    public int monotoneIncreasingDigits(int n) {
        String s = String.valueOf(n);
        char[] chars = s.toCharArray();
        int start = s.length();
        for (int i = s.length() - 2; i >= 0; i--) {
            if (chars[i] > chars[i + 1]) {
                chars[i]--;
                start = i+1;
            }
        }
        for (int i = start; i < s.length(); i++) {
            chars[i] = '9';
        }
        return Integer.parseInt(String.valueOf(chars));
    }
}
```

[968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/) 
本题是贪心和二叉树的一个结合
```
class Solution {
    int ans;
    Set<TreeNode> covered;
    public int minCameraCover(TreeNode root) {
        ans = 0;
        covered = new HashSet();
        covered.add(null);

        dfs(root, null);
        return ans;
    }

    public void dfs(TreeNode node, TreeNode par) {
        if (node != null) {
            dfs(node.left, node);
            dfs(node.right, node);

            if (par == null && !covered.contains(node) ||
                    !covered.contains(node.left) ||
                    !covered.contains(node.right)) {
                ans++;
                covered.add(node);
                covered.add(par);
                covered.add(node.left);
                covered.add(node.right);
            }
        }
    }
}
```

## 动态规划
动态规划主要分为以下几种问题  
1.动规基础	2.背包问题	3.打家劫舍	4.股票问题	5.子序列问题
### 第三十八天	
今天开始动态规划的内容，恰好今天的leetcode的每日一题也是一道dynamic programming的题目。
[509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/editorial/)   
经典的dynamic programming题目，也可以用递归来解决。
```
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i < n + 1; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```

[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)   
这也是经典的dynamic programming的问题。
```
class Solution {
    public int climbStairs(int n) {
        if(n <= 1) {
            return 1;
        }
        int[] dp = new int[n];
        dp[0] = 1;
        dp[1] = 2;
        for(int i = 2; i < n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n - 1];
    }
}
```

[746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)   
同样的经典的dp问题
```
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        if(n <= 1) return 0;
        if(n == 2) return Math.min(cost[0], cost[1]);
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i < n + 1; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[n];
    }
}
```

### 第三十九天
[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)   
从做题到现在做了不少动态规划的题目了，这个题相对比较简单，轻松拿下。
```
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        for(int i = 0; i < m; i ++) {
            for(int j = 0; j < n; j++) {
                if(j - 1 >= 0) {
                    dp[i][j] += dp[i][j - 1];
                }
                if(i - 1 >= 0) {
                    dp[i][j] += dp[i - 1][j];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)   
这个题可以说跟62基本一摸一样，多的一点点难度就是obstacle处不能走，为0.
```
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                    continue;
                }
                if(i - 1 >= 0) {
                    dp[i][j] += dp[i - 1][j];
                }
                if(j - 1 >= 0) {
                    dp[i][j] += dp[i][j - 1];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

### 第四十天
休息日

### 第四十一天
[343. Integer Break](https://leetcode.com/problems/integer-break/)   
这个整数拆分的题目，看到没什么思路，直接看题解了。
```
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1];
        dp[2] = 1;
        for(int i = 3; i < n + 1; i++) {
            for(int j = 1; j <= i / 2; j++) {
                dp[i] = Math.max(dp[i], Math.max((i - j) * j, dp[i - j] * j));
            }
        }
        return dp[n];
    }
}
```
这个题目还有个greedy的做法，就是把数字拆成3的倍数，余下的数跟3相乘是乘积最大的。但这个没看数学证明，可以记一下结论，不具有普遍性。
```
class Solution {
    public int integerBreak(int n) {
        if(n == 2) return 1;
        if(n == 3) return 2;
        if(n == 4) return 4;
        int result = 1;
        while(n > 4) {
            result *= 3;
            n -= 3;
        }
        result *= n;
        return result;
    }
}
```
[96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/description/)   
这个题自己花了不到10分钟ac了，主要是做了343题得到了一点点思路，感觉坐动态规划慢慢找到感觉了。
自己写的时候把dp[0]写成0了，这样在双for循环中需要加条件判断，其实可以写成dp[0]是1，因为其实没有node只有一种情况。
```
class Solution {
    public int numTrees(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        if(n == 2) return 2;
        int[] dp = new int[n + 1];
        dp[0] = 0; 
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i < n + 1; i++) {
            for(int j = 1; j <= i; j++) {
                if(j == 1) {
                    dp[i] += dp[i - j];
                    continue;
                }
                if(j == i) {
                    dp[i] += dp[j - 1]; 
                    continue;
                } 
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
}

// 改进版
class Solution {
    public int numTrees(int n) {
        if(n == 0) return 1;
        if(n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[0] = 1; 
        dp[1] = 1;
        for(int i = 2; i < n + 1; i++) {
            for(int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
}
```

### 第四十二天
今天开始背包问题，首先要理解的是[01背包问题]。
可以用[二维数组](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)和[一维的简化数组](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html)
[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)   
我觉得这个题的重点是怎么转换成背包问题来做。下面第一个是自己写的答案，可以看一下leetcode给的答案，直接用boolean数组来计算，更巧妙一些。
```
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num : nums) {
            sum += num;
        }
        if(sum % 2 == 1) {
            return false;
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for(int i = 0; i < nums.length; i++) {
            for(int j = target; j >= 0; j--) {
                if(nums[i] <= j) {
                    dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
                    if(dp[j] == target) return true;
                }
            }
        }
        return false;
    }
}

class Solution {
    public boolean canPartition(int[] nums) {
        if (nums.length == 0)
            return false;
        int totalSum = 0;
        // find sum of all array elements
        for (int num : nums) {
            totalSum += num;
        }
        // if totalSum is odd, it cannot be partitioned into equal sum subset
        if (totalSum % 2 != 0) return false;
        int subSetSum = totalSum / 2;
        boolean dp[] = new boolean[subSetSum + 1];
        dp[0] = true;
        for (int curr : nums) {
            for (int j = subSetSum; j >= curr; j--) {
                dp[j] |= dp[j - curr];
            }
        }
        return dp[subSetSum];
    }
}
```

### 第四十三天
[1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)   
这个题一开始其实没什么思路，看了卡哥的提示，说跟416题差不多，想到思路。
其实就是把石头分成两堆，让两堆尽可能相等。换言之，就是找到一组石头，让他越接近石头总weight的一半。也就转换成了背包问题，背包的容量是sum/2，物品的value是就是石头的weight，找到如何在有限空间内放入value最大的石头。
```
class Solution {
    public int lastStoneWeightII(int[] stones) {
        
        int sum = 0;
        for(int i = 0; i < stones.length; i++) {
            sum += stones[i];
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];
        dp[0] = 0;
        for(int i = 0; i < stones.length; i++) {
            for(int j = target; j >= 0; j--) {
                if(j - stones[i] >= 0) {
                    dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
                }      
            }
        }

        return Math.abs(sum - dp[target] - dp[target]);
    }
}
```
[494. Target Sum](https://leetcode.com/problems/target-sum/description/)   
这个题目很容易想到暴力的回溯法，卡哥说会超时，但实际在leetcode通过了。   
如果是背包问题是容量为i的背包，必须把i装满，问有几种装法，这就跟之前的容量为i，能放入的value最大是多少不太一样了。一开始就没什么思路。
那么现在dp[j]就变成了填满j的背包有几种方法，就变成了累加。从0-i的nums一个个试，这样累加，空间复杂度是O(n x m)
```
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) sum += nums[i];
	//如果target过大 sum将无法满足
        if ( target < 0 && sum < -target) return 0;
        if ((target + sum) % 2 != 0) return 0;
        int size = (target + sum) / 2;
        if(size < 0) size = -size;
        int[] dp = new int[size + 1];
        dp[0] = 1;
        for (int i = 0; i < nums.length; i++) {
            for (int j = size; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[size];
    }
}

```

[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)  
这个题如何主要难点在于把1和0的个数看成一个物品的两个维度，转换成一个01背包问题。
```
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        dp[0][0] = 0;
        for(int index = 0; index < strs.length; index++) {
            int numZero = 0;
            int numOne = 0;
            String s = strs[index];
            for(int i = 0; i < s.length(); i++) {
                if(s.charAt(i) == '1')  numOne++;
                if(s.charAt(i) == '0')  numZero++;
            }
            for(int i = m; i >= numZero; i--) {
                for(int j = n; j >= numOne; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - numZero][j - numOne] + 1);
                }
            }
        }
        return dp[m][n];
        
    }
}
```
## 完全背包问题
### 第四十四天
今天进入完全背包的专题。[理论概念看这里](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85)   
完全背包和01背包最主要的区别是，每个物品有无限多个。这就决定了如果用一维数组的话背包要从小到大遍历，因为每一个物品可以放无限多次。回忆一下，在01背包问题中，背包容量是从大到小遍历的，这样保证每个物品只最多存放一次。   

[518. Coin Change II](https://leetcode.com/problems/coin-change-ii/)    
这个是一个完全背包的组合问题。
```
// 如果求组合数就是外层for循环遍历物品，内层for遍历背包。

// 如果求排列数就是外层for遍历背包，内层for循环遍历物品。


class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for(int i = 0; i < coins.length; i++) {
            for(int j = 1; j <= amount; j++) {
                if(j - coins[i] >= 0) {
                    dp[j] = dp[j] + dp[j - coins[i]];
                }
            }
        }
        return dp[amount];
    }
}
```

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)   
这个是一个完全背包的排列问题。

```
// 如果求组合数就是外层for循环遍历物品，内层for遍历背包。

// 如果求排列数就是外层for遍历背包，内层for循环遍历物品。

class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for(int j = 1; j < target + 1; j++) {
            for(int i = 0; i < nums.length; i++) {
                if(j - nums[i] >= 0) {
                    dp[j] = dp[j] + dp[j - nums[i]];
                }
            }
        }
        return dp[target];
    }
}

```

### 第四十五天
今天用完全背包问题的思路来做几道题
[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)  
这个题之前做过，这次用完全背包思路的动归思想来做一下。按照完全背包的思想，这是一个组合的完全背包。


[322. Coin Change](https://leetcode.com/problems/coin-change/)  
这个题是排列的完全背包。
```
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for(int j = 0; j < coins.length; j++) {
            for(int i = 1; i <= amount; i++) {
                if(i - coins[j] >=0) {
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
}
```

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)  
感觉这个题跟上一道322. Coin change没有什么区别，唯一不同是需要自己找到最大值。也就是多一个开平方的步骤。
```
class Solution {
    public int numSquares(int n) {
        double max = Math.sqrt(n);
        int[] dp= new int[n + 1];
        Arrays.fill(dp, n);
        dp[0] = 0;
        for(int i = 1; i <= max; i++) {
            for(int j = 1; j < n + 1; j++) {
                if(j - i * i>=0) {
                    dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
                } 
            }
        }
        return dp[n];
    }
}
```

### 第四十六天
今天是多重背包问题，多重背包在leetcode上没有完全对应的题目。多重背包可以把m个数量的产品展开成m个不同的，这样就变成了01背包问题。
[139. Word Break](https://leetcode.com/problems/word-break/submissions/)   
这个题其实是完全背包问题，并不属于多重背包。感觉完全背包很容易看出来，但是遍历和dp的定义不是很好想。
```
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        HashSet<String> set = new HashSet<>(wordDict);
        boolean[] valid = new boolean[s.length() + 1];
        valid[0] = true;

        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i && !valid[i]; j++) {
                if (set.contains(s.substring(j, i)) && valid[j]) {
                    valid[i] = true;
                }
            }
        }

        return valid[s.length()];
    }
}
```
### 第四十七天

### 第四十八天
[198. House Robber](https://leetcode.com/problems/house-robber/description/)   
打家劫舍的经典题目
```
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n < 2) {
            return nums[0];
        }
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(dp[0], nums[1]);
        for(int i = 2; i < n; i++) {
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[n - 1];
    }
}
```

[213. House Robber II](https://leetcode.com/problems/house-robber-ii/)  
与上一个题很像，但是这个成环了。所以可以分成两种情况，一种不包含下标为0，一种不包含下下标为n-1，这样就跟198一样了。
```
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int len = nums.length;
        if (len == 1)
            return nums[0];
        return Math.max(robAction(nums, 0, len - 1), robAction(nums, 1, len));
    }

    int robAction(int[] nums, int start, int end) {
        int x = 0, y = 0, z = 0;
        for (int i = start; i < end; i++) {
            y = z;
            z = Math.max(y, x + nums[i]);
            x = y;
        }
        return z;
    }
}
```

[337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)   
树形dp，还是很重要的。
```
class Solution {
    public int rob(TreeNode root) {
        int[] res = robAction1(root);
        return Math.max(res[0], res[1]);
    }

    int[] robAction1(TreeNode root) {
        int res[] = new int[2];
        if (root == null)
            return res;

        int[] left = robAction1(root.left);
        int[] right = robAction1(root.right);

        res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        res[1] = root.val + left[0] + right[0];
        return res;
    }
}
```

### 第四十九天
[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)   
感觉这个题用动态规划做不是很好，仔细思考一下，一个二维数组储存，其实就是一列是最小值，一列是差的最大值。跟贪心是一样的，还浪费了空间。
```
// 动态规划
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][2];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for(int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], -prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], prices[i] + dp[i][0]);
        }
        return dp[prices.length - 1][1];
    }
}

// 贪心
class Solution {
    public int maxProfit(int[] prices) {
        int buyDay = Integer.MAX_VALUE;
        int ans = 0;
        for(int i = 0; i < prices.length; i++) {
            if(prices[i] < buyDay) {
                buyDay = prices[i];
            }
            ans = Math.max(ans, prices[i] - buyDay);
        }
        return ans;
    }
}

```

[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)   
这个题也是dp和greedy两种思路
```
// dp
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[] dp = new int[n];
        dp[0] = 0;
        for(int i = 1; i < n; i++) {
            int dif = prices[i] - prices[i - 1];
            dp[i] = Math.max(dp[i - 1], dp[i - 1] + dif);
        }
        return dp[n - 1];
    }
}

// Greedy
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        for(int i = 1; i < prices.length; i++) {
            int dif = prices[i] - prices[i - 1];
            if(dif > 0) {
                profit += dif;
            }
        }
        return profit;
    }
}


```


### 第五十天
[123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)   
这个题的难度比较高，因为限制了只能买卖两次。
```
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        // 边界判断, 题目中 length >= 1, 所以可省去
        if (prices.length == 0) return 0;

        /*
         * 定义 5 种状态:
         * 0: 没有操作, 1: 第一次买入, 2: 第一次卖出, 3: 第二次买入, 4: 第二次卖出
         */
        int[][] dp = new int[len][5];
        dp[0][1] = -prices[0];
        // 初始化第二次买入的状态是确保 最后结果是最多两次买卖的最大利润
        dp[0][3] = -prices[0];

        for (int i = 1; i < len; i++) {
            dp[i][1] = Math.max(dp[i - 1][1], -prices[i]);
            dp[i][2] = Math.max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
            dp[i][3] = Math.max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
            dp[i][4] = Math.max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
        }

        return dp[len - 1][4];
    }
}
```

[188. Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)  
觉得这个题跟123难度差不多，只需要多一个for循环循环一下k次操作就可以。
```
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][k * 2 + 1];
        dp[0][0] = 0;
        for(int i = 1; i < k * 2 + 1; i++) {
            if(i % 2 != 0) {
                dp[0][i] = -prices[0];
            }
        }
        for(int i = 1; i < n; i++) {
            for(int j = 1; j < k * 2 + 1; j++) {
                if(j % 2 != 0) {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1] - prices[i]);
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1] + prices[i]);
                }
            }
        }

        return dp[n - 1][k * 2];
    }
}
```
### 第五十一天

[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)   
这个题的难度主要是在于在卖出股票后有一天的冷冻期。  
为了好理解，carl哥在讲解的时候用了四种状态，leetcode的原题用了三种状态来做的。四种状态的思路很清晰，三种状态如果画出state machine其实也是很好理解并写出代码的。在这里附上三种答案。
1.四种状态的dp数组  
2.三种状态的dp数组
3.三种状态不用dp数组
```
class Solution {
    public int maxProfit(int[] prices) {
      int n = prices.length;
        int[][] dp = new int[prices.length][4];
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        dp[0][2] = 0;
        dp[0][3] = 0;
        for(int i = 1; i < n; i++) {
          dp[i][0] = Math.max(dp[i - 1][0], Math.max(dp[i - 1][1] - prices[i], dp[i - 1][3] - prices[i]));
          dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][3]);
          dp[i][2] = dp[i][0] + prices[i];
          dp[i][3] = dp[i - 1][2];
        }
        return Math.max(dp[n -1][1],Math.max(dp[n - 1][3], dp[n - 1][2]));
    }
}

class Solution {
  public int maxProfit(int[] prices) {
    int n = prices.length;
    int[][] dp = new int[n][3];
    dp[0][0] = -prices[0];
    dp[0][1] = 0;
    dp[0][2] = 0;

    for (int i = 1; i < n; i++) {
      dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][2] - prices[i]);
      dp[i][1] = dp[i - 1][0] + prices[i];
      dp[i][2] = Math.max(dp[i - 1][1], dp[i - 1][2]);
    }

    return Math.max(dp[n - 1][1], dp[n - 1][2]);
  }
}

class Solution {
  public int maxProfit(int[] prices) {

    int sold = Integer.MIN_VALUE, held = Integer.MIN_VALUE, reset = 0;

    for (int price : prices) {
      int preSold = sold;

      sold = held + price;
      held = Math.max(held, reset - price);
      reset = Math.max(reset, preSold);
    }

    return Math.max(sold, reset);
  }
}
```
[714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)   
这个题多了个手续费的步骤，记得要减去手续费。因为有了手续费，当天的买卖并不是非负收益，所以需要取sold的max值。写一个不占用空间的方法，跟dp数组是一样的。
```
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int hold = Integer.MIN_VALUE;
        int sold = 0;
        
        for (int price : prices) {
            int oldHold = hold;
            hold = Math.max(hold, sold - price - fee);
            sold = Math.max(sold, oldHold + price);
        }
        
        return sold;
    }
}
```

### 第五十二天
[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)   
```
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int ans = 1;
        Arrays.fill(dp, 1);
        for(int i = 1; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                }
            }
            ans = Math.max(dp[i], ans);
        }
        return ans;
    }
}
```
[674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)   
这个题的正解应该是用双指针，而且很容易想到。用dp的话很消耗空间，思考一下和题目300的区别就行了。
```
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        int ans = 1;
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            if(nums[i] > nums[i - 1]) {
                dp[i] = dp[i - 1] + 1;
            } else {
                dp[i] = 1;
            }
            ans = Math.max(dp[i], ans);
        }
        return ans;
    }
}
```

[718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)  
这个题目还是很有难度的，特别是dp中如何可以巧妙的把二维数组压缩成一维的滚动数组。
贴上[carl哥的讲解](https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html)  

```
// 二维dp数组
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int result = 0;
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        
        for (int i = 1; i < nums1.length + 1; i++) {
            for (int j = 1; j < nums2.length + 1; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    result = Math.max(result, dp[i][j]);
                }
            }
        }
        
        return result;
    }
}

// 滚动数组
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int[] dp = new int[nums2.length + 1];
        int result = 0;

        for (int i = 1; i <= nums1.length; i++) {
            for (int j = nums2.length; j > 0; j--) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[j] = dp[j - 1] + 1;
                } else {
                    dp[j] = 0;
                }
                result = Math.max(result, dp[j]);
            }
        }
        return result;
    }
}
```

### 第五十三天
[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)  
这个题与718的差别在于，718是subarray，这个题是subsequence。
```
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        char[] ary1 = text1.toCharArray();
        char[] ary2 = text2.toCharArray();
        int[] dp = new int[ary2.length + 1];
        for(int i = 1; i <= ary1.length; i++) {
            int pre = dp[0];
            for(int j = 1; j <= ary2.length; j++) {
                int cur = dp[j];
                if(ary1[i - 1] == ary2[j - 1]) {
                    dp[j] = pre + 1;
                } else {
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }
                pre = cur;
            }
        }
        return dp[ary2.length];
    }
}
```

[1035. Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/)  
本题最关键的地方是想明白两个数组连线不想交，其实就是找两个数组的subsequence。这个题就转换成了跟上个题1143一样了。
```
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int n2 = nums2.length;
        int[] dp = new int[n2 + 1];
        
        for(int i = 1; i <= n1; i++) {
            int pre = dp[0];
            for(int j = 1; j <= n2; j++) {
                int cur = dp[j];
                if(nums1[i - 1] == nums2[j - 1]) {
                    dp[j] = pre + 1;
                } else {
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }
                pre = cur;
            }
        }
        return dp[n2];
    }
}
```

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)   
以前用greedy做过一次，这次用dp来做
```
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int ans = dp[0];
        for(int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(nums[i], dp[i - 1] + nums[i]);
            ans = Math.max(ans , dp[i]);
        }
        return ans;
    }
}
```
### 第五十四天
周末休息

### 第五十五天
[392. Is Subsequence](https://leetcode.com/problems/is-subsequence/description/)   
今天恰好每日一题是[712. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/)，与这个很像，难度大很多。
```
class Solution {
    public boolean isSubsequence(String s, String t) {
        if(t.length() < s.length()) {
            return false;
        }
        int loc = -1;
        int length = 0;
        for(int i = 0; i < s.length(); i++) {
            for(int j = loc + 1; j < t.length(); j++) {
                if(s.charAt(i) == t.charAt(j)) {
                    loc = j;
                    length++;
                    break;
                }
            }
        }
        return length == s.length() ? true : false;
    }
}
```

[115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/description/)   
这个题目我用了剪枝的回溯还是超时了。
看了一下carl哥的思路，这个题目还是挺有难度的，dp的递推跳过一个字母的思路比较难想。
```
class Solution {
    public int numDistinct(String s, String t) {
        int[][] dp = new int[s.length() + 1][t.length() + 1];
        for (int i = 0; i < s.length() + 1; i++) {
            dp[i][0] = 1;
        }
        
        for (int i = 1; i < s.length() + 1; i++) {
            for (int j = 1; j < t.length() + 1; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        
        return dp[s.length()][t.length()];
    }
}
```

### 第五十六天
[583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/description/)  
这个题目同时删除两个字符串有两种dp的思路。  
第一种，用二维的dp数组记录i和j需要删除的次数。  
第二种，像1143最长子序列那样，记录两个字符串的共有长度，最后减去共有长度就是需要删除的次数。 
```
// 用dp数组记录删除的次数
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp = new int[word1.length() + 1][word2.length() + 1];
        for(int i = 0; i <= word1.length(); i++) {
            dp[i][0] = i;
        }
        for(int i = 0; i <= word2.length(); i++) {
            dp[0][i] = i;
        }
        for(int i = 0; i < word1.length(); i++) {
            for(int j = 0; j < word2.length(); j++) {
                if(word1.charAt(i) == word2.charAt(j)) {
                    dp[i + 1][j + 1] = dp[i][j];
                } else {
                    dp[i + 1][j + 1] = Math.min(dp[i][j + 1], dp[i + 1][j]) + 1;
                }
            }
        }
        return dp[word1.length()][word2.length()];
    }
}

//用dp数组记录共有的长度
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length();
        int len2 = word2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];
        for(int i = 0; i < len1; i++) {
            for(int j = 0 ; j < len2; j++) {
                if(word1.charAt(i) == word2.charAt(j)) {
                    dp[i + 1][j + 1] = dp[i][j] + 1;
                } else {
                    dp[i + 1][j + 1] = Math.max(dp[i][j + 1], dp[i + 1][j]);
                }
            }
        }
        return len1 + len2 - dp[len1][len2] * 2;
        
    }
}

```

[72. Edit Distance](https://leetcode.com/problems/edit-distance/)   
前面铺垫了那么多，终于来到了最终问题，编辑距离。  
推dp的方法是一样的，只不过这次的操作比以前复杂，需要好好思考一下。
```
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length();
        int len2 = word2.length();
        int[][] dp = new int[len1 + 1][len2 + 2];
        for(int i = 0; i <= len1; i++) {
            dp[i][0] = i;
        }
        for(int j = 0; j <= len2; j++) {
            dp[0][j] = j;
        }
        for(int i = 0; i < len1; i++) {
            for(int j = 0; j < len2; j++) {
                if(word1.charAt(i) == word2.charAt(j)) {
                    dp[i + 1][j + 1] = dp[i][j];
                } else {
                    dp[i + 1][j + 1] = Math.min(dp[i][j], Math.min(dp[i + 1][j], dp[i][j + 1])) + 1;
                }
            }
        }
        return dp[len1][len2];
    }
}
```

### 第五十七天
[647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/)   
回文子串，如果用暴力方法是O(n^3)。  
dp的简化思路是在O(1)的时间内找到下标i-j的字符串是不是回文，而i-j跟(i-1) - (j-1)有关系。这样思路就明确了。
```
class Solution {
    public int countSubstrings(String s) {
        boolean[][] dp = new boolean[s.length()][s.length()];
        int ans = 0;
        for(int i = 0; i < s.length(); i++) {
            for(int j = 0; j <= i; j++) {
                dp[i][j] = true;
            }
            ans++;
        }
        for(int i = s.length() - 1; i >= 0; i--) {
            for(int j = s.length() - 1; j > i; j--) {
                if(s.charAt(j) != s.charAt(i)) {
                    dp[i][j] = false;
                } else {
                    dp[i][j] = dp[i + 1][j - 1];
                }
                if(dp[i][j])    ans++;
            }
        }
        return ans;
        
    }
}
```

