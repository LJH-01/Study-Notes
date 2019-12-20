# 剑指offer第14题-链表中倒数第K个节点

## 题目描述

输入一个链表，输出该链表中倒数第k个结点。

---

最佳代码：Java代码，通过校验。代码思路如下：两个指针，先让第一个指针和第二个指针都指向头结点，然后再让第一个指正走(k-1)步，到达第k个节点。然后两个指针同时往后移动，当第一个结点到达末尾的时候，第二个结点所在位置就是倒数第k个节点了。。

---

我的慢速代码

---

/*

public class ListNode {

    int val;

    ListNode next = null;

    ListNode(int val) {

        this.val = val;

    }

}*/

public class Solution {

    public ListNode FindKthToTail(ListNode head,int k) {

        int n=0;

        ListNode node = new ListNode(1);

        node = head;

        while(node !=null){

            n++;

            node=node.next;

        }// n is the count of node

        if(k>n) return null;

        k=n-k+1;

        for(int i=1;i<=k-1;i++){

            head = head.next;

        }

        return head;

    }

}
