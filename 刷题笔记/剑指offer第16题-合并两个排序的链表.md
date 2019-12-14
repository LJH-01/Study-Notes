# 剑指offer第16题-合并两个排序的链表

## 题目描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

---

我的慢速代码 这道题卡在，if-else if- else if--的判断上

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

    public ListNode Merge(ListNode list1,ListNode list2) {

        ListNode memory = new ListNode(0);//用来记住头

        ListNode tempnode = new ListNode(0);

        if(list1==null && list2==null) memory = null;

        else if(list1!=null || list2!=null){

            if(list1!=null && list2!=null){

                if(list1.val<=list2.val) {

                    tempnode = list1;

                    list1 = list1.next;

                }

                else{

                    tempnode = list2;

                    list2 = list2.next;

                }

            }

            else if(list1!=null && list2==null){

                tempnode = list1;

                list1= list1.next;

            }

            else if(list2!=null && list1==null){

                tempnode = list2;

                list2= list2.next;

            }

            memory = tempnode;

        }//关于头的处理

        while(list1!=null || list2!=null){

            if(list1!=null && list2!=null){

                if(list1.val<=list2.val) {

                    tempnode.next = list1;

                    list1 = list1.next;

                }

                else{

                    tempnode.next = list2;

                    list2 = list2.next;

                }

                tempnode = tempnode.next;

            }else if(list1 !=null && list2==null){

                tempnode.next = list1;

                list1 = list1.next;

                tempnode = tempnode.next;

            }else if(list1 ==null && list2!= null){

                tempnode.next =list2;

                list2 = list2.next;

                tempnode = tempnode.next;

            }

        }

        return memory;

    }

}
