# 剑指offer第3题-从尾到头打印链表

时间限制：1秒 空间限制：32768K 热度指数：927004

本题知识点： [链表](https://www.nowcoder.com/questionCenter?questionTypes=000100&mutiTagIds=580)

## 题目描述

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

---

/**

*    public class ListNode {

*        int val;

*        ListNode next = null;

*

*        ListNode(int val) {

*            this.val = val;

*        }

*    }

*

*/

用递归：

import java.util.ArrayList;

public class Solution {

    ArrayList<Integer> arrayList=new ArrayList<Integer>();

    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {

        if(listNode!=null){

            this.printListFromTailToHead(listNode.next);

            arrayList.add(listNode.val);

        }

        return arrayList;

    }

}

用栈：

import java.util.ArrayList;

import java.util.Stack;

public class Solution {

    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {

        Stack<Integer> stack=new Stack<Integer>();

        while(listNode!=null){

            stack.push(listNode.val);

            listNode=listNode.next;     

        }

        ArrayList<Integer> list=new ArrayList<Integer>();

        while(!stack.isEmpty()){

            list.add(stack.pop());

        }

        return list;

    }

}
