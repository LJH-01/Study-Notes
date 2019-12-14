# 剑指offer第56题-删除链表中重复的节点

## 题目描述

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

---

链表要删除前一个节点就很烦，有一个技巧是在头部之前添加一个新的节点，防止头结点被删造成无穷无尽的重复讨论。代码：

/*

public class ListNode {

    int val;

    ListNode next = null;

    ListNode(int val) {

        this.val = val;

    }

}

*/

public class Solution {

    public ListNode deleteDuplication(ListNode pHead){

        ListNode addition = new ListNode(0);

        addition.next = pHead;

        ListNode ppre = addition;

        ListNode pre = pHead;

        if(pre == null) return pre;

        ListNode cur = pre.next;

        if(cur == null) return pre;

        boolean flag = false;

        while(cur!=null){

            if(cur.val == pre.val){

                cur = cur.next;

                flag = true;

            }

            else{

                if(flag){

                    pre = cur;

                    ppre.next = pre;

                    cur = cur.next;

                    flag = false;

                }

                else{

                    ppre = pre;

                    pre = cur;

                    cur = cur.next;

                }

            }

        }

        if(flag) ppre.next = null;

        return addition.next;

    }

}
