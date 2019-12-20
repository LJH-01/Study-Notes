# 剑指offer第36题-两个链表的第一个公共节点

## 题目描述

输入两个链表，找出它们的第一个公共结点。

---

先对两个链表遍历，算出两个链表的长度。都是单链表，如果有公共节点，公共节点后面的元素都相等，所以，只可能是公共节点之前的节点不相同，所以，先计算出两个链表的长度差，让比较长的那个先往前走相应的步数，然后开始同时走，判断他们是否相同。若任意一个遍历到了头，说明没有公共节点，返回null

---

/*

public class ListNode {

    int val;

    ListNode next = null;

    ListNode(int val) {

        this.val = val;

    }

}*/

import java.util.LinkedList;

public class Solution {

    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {

        int count1 = 0,count2 =0;

        if(pHead1==null || pHead2==null) return null;

        ListNode current = pHead1;

        while(current!=null){

            count1++;

            current = current.next;

        }

        current = pHead2;

        while(current!=null){

            count2++;

            current=current.next;

        }

        if(count1>count2){

            current = pHead1;

            for(int i=1;i<=count1-count2;i++){

                current = current.next;

            }

            pHead1=current;

        }else if(count2>count1){

            current = pHead2;

            for(int i=1;i<=count2-count1;i++){

                current = current.next;

            }

            pHead2= current;

        }

        current = null;

        while(pHead1!=null){

            if(pHead1==pHead2) {

                current = pHead1;

                break;

            }else{

                pHead1=pHead1.next;

                pHead2=pHead2.next;

            }

        }

        return current;

    }

}
