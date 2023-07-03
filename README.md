# 代码随想录刷题总结
## 目录
[第一天](#第一天)     [第二天](#第二天)     [第三天](#第三天)     [第四天](#第四天) [第五天](#第五天) [第六天](#第六天) [第七天](#第七天)
[第八天](#第八天)     [第九天](#第九天)     [第十天](#第十天)     [第十一天](#第十一天)    [第十二天](#第十二天)    [第十三天](#第十三天)
[第十四天](#第十四天)    [第十五天](#第十五天) [第十六天](#第十六天) [第十七天](#第十七天)    [第十八天](#第十八天)
[第十九天](#第十九天)    [第二十天](#第二十天)      [第二十一天](#第二十一天)	[第二十二天](#第二十二天)	[第二十三天](#第二十三天)
[第二十四天](#第二十四天) 	[第二十五天](#第二十五天) [第二十六天](#第二十六天)	[第二十七天](#第二十七天)

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
