![[Pasted image 20230911104732.png]]当不确定答案时候，先说猜想。

是不是s1没赋值
是不是catch的顺序

问一下，他肯定就继续。否定就排除法了啊

回答每个问题，对方问完问题后都停几秒钟再回答。


刚开始说不出思路，然后说我可以边写代码边解释吗

confused / 重新说一遍问题

对方声音突然变小了，重新说一遍





```java
/* Online Java Compiler and Editor

You are given an array of k linked-lists lists, each linke-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
1 4 5 -> list -> sort -> res
*/

import java.util.*;

class ListNode{
    int val;
    ListNode next;
    ListNode(){}
    ListNode(int val) {
        this.val = val;
    }
    ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}

public class HelloWorld{
    
    public static ListNode merge(ListNode[] lists) {
        List<Integer> list = new ArrayList<>();
        
        for (ListNode ln: lists) {
            while (ln != null) {
                list.add(ln.val);
                System.out.print(ln.val);
                ln = ln.next;
            }
        }
        
        Collections.sort(list);
        
        ListNode head = new ListNode(0);
        ListNode cur = head;
        for(int val : list) {
            ListNode tmp = new ListNode(val);
            cur.next = tmp;
            cur = cur.next;
        }
        
        cur.next = null;
        return head.next;
    }
    
     public static void main(String []args){
        System.out.println("Hello, World!");
        //Input: lists = [[1,4,5],[1,3,4],[2,6]]
        //Output: [1,1,2,3,4,4,5,6]
        ListNode[] lists = new ListNode[3];
        ListNode l1 = new ListNode(1, new ListNode(4, new ListNode(5)));
        ListNode l2 = new ListNode(1, new ListNode(3, new ListNode(4)));
        // ListNode l3 = new ListNode(2, new ListNode(6));
        ListNode l3 = new ListNode();
        while (l3!=null){
            System.out.print(l3.val);
            l3=l3.next;
        }
        // [1,1,3,4,4,5]
        
        lists[0] = l1;
        lists[1] = l2;
        lists[2] = l3;
        
        ListNode result = merge(lists);
        while (result != null) {
            System.out.print(result.val);
            result = result.next;
        }
        
     }
}
```