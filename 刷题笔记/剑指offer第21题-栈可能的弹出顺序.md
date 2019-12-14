# 剑指offer第21题-栈可能的弹出顺序

## 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

---

/**

*解法：模拟法

*/

package solution;

import java.util.Stack;

/**

* @author TQR

* 2019年8月25日

*/

public class Solution_Offer21 {

    public static boolean IsPopOrder(int [] pushA,int [] popA) {

        if(pushA.length!=popA.length)return false;//长度不等直接判错

        //下面模拟该过程看是否可以获得出栈顺序。

        boolean result = false;

        Stack<Integer> stack = new Stack<Integer>();

        int pushOrder=0,popOrder=0;//初始入栈出栈的下标都是0

        stack.push(pushA[pushOrder]);//先将第一个数入栈

        while(pushOrder<popA.length && popOrder<popA.length){//遍历出栈序列

            if(stack.peek()==popA[popOrder]) {//如果当前栈顶元素和当前出栈元素相等

                int k=stack.pop();//当前元素出栈

                System.out.println(k+"出栈");

                //pushOrder +=1;

                popOrder +=1;

            }else if(stack.peek() != popA[popOrder]) {//栈顶元素不等于当前出栈元素

                pushOrder +=1;

                if(pushOrder<pushA.length) {

                    stack.push(pushA[pushOrder]);

                    System.out.println(pushA[pushOrder]+"入栈");

                }                

            }

            System.out.println("popOrder:"+popOrder+"  pushOrder:"+pushOrder);

            if(popOrder == popA.length && pushOrder==pushA.length-1) result = true;

        }

        return result;

    }

    public static void main (String []args) {

        System.out.println("program is started ··· Let's go!");

        int [] pushA = {1,2,3,4,5}, popA = {4,3,5,1,2};

        System.out.println(IsPopOrder(pushA,popA));

    }

}
