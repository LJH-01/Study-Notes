# 剑指offer第60题-把二叉树打印成多行

## 题目描述

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

---

解析

和上题类似，都是属于二叉树的层次遍历，纯层次遍历----》一个队列

之字形遍历 用两个栈

按层打印，用两个队列  （我的方法）

import java.util.ArrayList;

import java.util.LinkedList;

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

    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {

        ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();

        if(pRoot == null) return list;

        LinkedList<TreeNode> queue1 = new LinkedList<TreeNode>();

        LinkedList<TreeNode> queue2 = new LinkedList<TreeNode>();

        queue1.offer(pRoot);

        while(!queue1.isEmpty() || !queue2.isEmpty()){

            ArrayList<Integer> listtemp = new ArrayList<Integer>();

            if(!queue1.isEmpty()){

                while(!queue1.isEmpty()){

                    TreeNode p = queue1.pollFirst();

                    listtemp.add(p.val);

                    if(p.left!=null) queue2.offer(p.left);

                    if(p.right!=null) queue2.offer(p.right);

                }

                list.add(listtemp);

            }else {

                while(!queue2.isEmpty()){

                    TreeNode p = queue2.pollFirst();

                    listtemp.add(p.val);

                    if(p.left!=null) queue1.offer(p.left);

                    if(p.right!=null) queue1.offer(p.right);

                }

                list.add(listtemp);

            }

        }

        return list;

    }

}
