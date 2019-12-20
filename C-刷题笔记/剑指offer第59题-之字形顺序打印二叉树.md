# 剑指offer第59题-之字形顺序打印二叉树

## 题目描述

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

---

解析：利用栈的特性，使用两个栈，仔细考虑一下之字形顺序即可

import java.util.ArrayList;

import java.util.Stack;

/*

public class TreeNode {

    int val = 0;

    TreeNode left = null;

    TreeNode right = null;

    public TreeNode(int val) {

        this.val = val;

    }

}

*/

public class Solution {

    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {

        ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();

        if(pRoot == null) return list;

        Stack<TreeNode> s1 = new Stack<TreeNode>();

        Stack<TreeNode> s2 = new Stack<TreeNode>();

        s1.push(pRoot);

        int layer = 1;

        while(!s1.empty() || !s2.empty()){

ArrayList<Integer> listtemp = new ArrayList<Integer>();

            if(layer%2==1){

                while(!s1.empty()){

                    TreeNode p = s1.pop();

                    listtemp.add(p.val);

if(p.left!=null) s2.push(p.left);

                    if(p.right!=null) s2.push(p.right);

                }

                list.add(listtemp);

                layer++;

            }else{

                while(!s2.empty()){

                    TreeNode p = s2.pop();

                    listtemp.add(p.val);

if(p.right!=null) s1.push(p.right);

                    if(p.left!=null) s1.push(p.left);

                }

                list.add(listtemp);

                layer++;

            }

        }

        return list;

    }

}
