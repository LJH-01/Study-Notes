# 剑指offer第15题-反转链表

## 题目描述

输入一个链表，反转链表后，输出新链表的表头。

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

    public ListNode ReverseList(ListNode head) {

        ListNode node1 = new ListNode(0);

        ListNode node2 = new ListNode(0);

        ListNode tempNode = new ListNode(0);

        if(head==null) node2 = null;

        else if(head.next == null) node2 = head;

        else{

            node1 = head;

            node2 = head.next;

            node1.next = null;

            tempNode = node2.next;

            node2.next = node1;

            while(tempNode != null){

                node1 =node2;

                node2 = tempNode;

                tempNode = node2.next;

                node2.next=node1;

            }

        }

        return node2;

    }

}

---

网友的代码：优化的更简单但是思路一样

---

public class Solution {

    public ListNode ReverseList(ListNode head) {

        if(head==null)

            return null;

        //head为当前节点，如果当前节点为空的话，那就什么也不做，直接返回null；

        ListNode pre = null;

        ListNode next = null;

        //当前节点是head，pre为当前节点的前一节点，next为当前节点的下一节点

        //需要pre和next的目的是让当前节点从pre->head->next1->next2变成pre<-head next1->next2

        //即pre让节点可以反转所指方向，但反转之后如果不用next节点保存next1节点的话，此单链表就此断开了

        //所以需要用到pre和next两个节点

        //1->2->3->4->5

        //1<-2<-3 4->5

        while(head!=null){

            //做循环，如果当前节点不为空的话，始终执行此循环，此循环的目的就是让当前节点从指向next到指向pre

            //如此就可以做到反转链表的效果

            //先用next保存head的下一个节点的信息，保证单链表不会因为失去head节点的原next节点而就此断裂

            next = head.next;

            //保存完next，就可以让head从指向next变成指向pre了，代码如下

            head.next = pre;

            //head指向pre后，就继续依次反转下一个节点

            //让pre，head，next依次向后移动一个节点，继续下一次的指针反转

            pre = head;

            head = next;

        }

        //如果head为null的时候，pre就为最后一个节点了，但是链表已经反转完毕，pre就是反转后链表的第一个节点

        //直接输出pre就是我们想要得到的反转后的链表

        return pre;

    }

}
