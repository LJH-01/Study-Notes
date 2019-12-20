# 剑指offer第62题-二叉搜索树的第k个节点

## 题目描述

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

---

解析：这道题用递归遍历反而非常容易弄混，我认为这道题就是考察非递归中序遍历二叉树

别人的代码

import java.util.Stack;

public class Solution {

TreeNode KthNode(TreeNode root, int k){

        if(root==null||k==0)

            return null;

        Stack<TreeNode> stack = new Stack<TreeNode>();

        int count = 0;

        TreeNode node = root;

        do{

            if(node!=null){

                stack.push(node);

                node = node.left;

            }else{

                node = stack.pop();

                count++;

                if(count==k)

                    return node;

                node = node.right;

            }

        }while(node!=null||!stack.isEmpty());

        return null;

    }

}
