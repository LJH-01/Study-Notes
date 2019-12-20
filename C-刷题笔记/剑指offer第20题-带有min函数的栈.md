# 剑指offer第20题--带有min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

---

题挺简单的，借用一个辅助栈，就完事，关键在理解题意和  复习了Stack中pop() push() peek() 等方法的用法。

import java.util.Stack;

public class Solution {

    Stack<Integer> mainStack = new Stack<Integer>();

    Stack<Integer> minStack = new Stack<Integer>();

    int min = Integer.MAX_VALUE;

    public void push(int node) {

        mainStack.push(node);

        if(node<min){

            min = node;

            minStack.push(min);

        }

        else minStack.push(min);

    }

    public void pop() {

        mainStack.pop();

        minStack.pop();

    }

    public int top() {

        return mainStack.peek();

    }

    public int min() {

        return minStack.peek();

    }

}
