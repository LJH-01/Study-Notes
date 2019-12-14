# 剑指offer第26题-二叉搜索树和双向链表--

## 题目描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向

---

高赞答案有些绕，[https://www.nowcoder.com/questionTerminal/947f6eb80d944a84850b0538bf0ec3a5](https://www.nowcoder.com/questionTerminal/947f6eb80d944a84850b0538bf0ec3a5)

我的答案借助了一个队列，应该说时间复杂度和空间复杂度都有提升，但是思考复杂度就很简单了

我的代码：

import java.util.LinkedList;

public class Solution {

    private LinkedList<TreeNode> queue = new LinkedList<TreeNode>();

    public TreeNode Convert(TreeNode pRootOfTree) {

        if(pRootOfTree == null) return null;

        midOrderTravel(pRootOfTree);//中序遍历获取队列

        TreeNode rootOfList = queue.pollFirst();//获取头

        rootOfList.left=null;

        rootOfList.right = null;

        TreeNode current = rootOfList;

        TreeNode pre = null;

        while(!queue.isEmpty()){

            current.right = queue.peekFirst();

            pre = current;

            current = queue.pollFirst();

            current.left=pre;

            current.right=null;

        }

        return rootOfList;

    }

    public void midOrderTravel(TreeNode root) {

        if(root==null) return ;//递归退出

        midOrderTravel(root.left);

        queue.offer(root);

        midOrderTravel(root.right);

    }

}
