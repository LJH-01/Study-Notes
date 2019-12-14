# 剑指offer第55题-找链表环入口

## 题目描述

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

---

我的代码：直接hashSet一波搞定：

import java.util.HashSet;

public class Solution {

    public ListNode EntryNodeOfLoop(ListNode pHead)

    {

        HashSet<ListNode> hs = new HashSet<ListNode>();

        while(pHead!=null){

            if(hs.add(pHead)){

                pHead = pHead.next;

            }

            else return pHead;

        }

        return null;

    }

}

别人很厉害很简练的方法：

1. 第一步，找环中相汇点。分别用p1，p2指向链表头部，p1每次走一步，p2每次走二步，直到p1==p2找到在环中的相汇点。

2. 第二步，找环的入口。分析：再让p2指向链表头部，p1位置不变，p1,p2每次走一步直到p1==p2; 此时p1指向环的入口。

public class Solution {

    ListNode EntryNodeOfLoop(ListNode pHead){

        if(pHead == null || pHead.next == null)

            return null;

        ListNode p1 = pHead;

        ListNode p2 = pHead;

        while(p2 != null && p2.next != null ){

            p1 = p1.next;

            p2 = p2.next.next;

            if(p1 == p2){

                p2 = pHead;

                while(p1 != p2){

                    p1 = p1.next;

                    p2 = p2.next;

                }

                if(p1 == p2)

                    return p1;

            }

        }

        return null;

    }

}
