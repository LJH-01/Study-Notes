# 剑指offer第5题-两个栈实现队列

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

---

import java.util.Stack;

public class Solution {

    Stack<Integer> stack1 = new Stack<Integer>();

    Stack<Integer> stack2 = new Stack<Integer>();//栈2保存队列序

    public void push(int node) {

        //先把stack2给到栈一，使栈1始终保持栈序

        while(!stack2.empty()){

            stack1.push(stack2.pop());

        }

        stack1.push(node);//对于栈序push

        while(!stack1.empty()){

            stack2.push(stack1.pop());

        }

    }

    public int pop() {

        return stack2.pop();

    }

}
