# 剑指offer第58题-对称的二叉树

## 题目描述

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

---

解析

public class Solution {

    boolean isSymmetrical(TreeNode pRoot)

    {

        if(pRoot == null) return true;

        return func(pRoot.left,pRoot.right);

    }

    private static boolean func(TreeNode left,TreeNode right){

        if(left == null && right == null) return true;

        if((left == null && right != null) || (left != null && right == null)) return false;

    //递归退出条件

    //本次递归要做的事情。

        return left.val==right.val && func(left.left,right.right) && func(left.right,right.left);

    }

}
