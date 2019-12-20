# 剑指offer第4题-重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

---

import java.util.*;

/**

* Definition for binary tree

* public class TreeNode {

*     int val;

*     TreeNode left;

*     TreeNode right;

*     TreeNode(int x) { val = x; }

* }

*/

public class Solution {

    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {

       if(pre.length == 0||in.length == 0){

            return null;

        }

        TreeNode node = new TreeNode(pre[0]);

        for(int i = 0; i < in.length; i++){

            if(pre[0] == in[i]){

                node.left = reConstructBinaryTree(Arrays.copyOfRange(pre, 1, i+1), Arrays.copyOfRange(in, 0, i));

                node.right = reConstructBinaryTree(Arrays.copyOfRange(pre, i+1, pre.length), Arrays.copyOfRange(in, i+1,in.length));

            }

        }

        return node;

    }

}

---

我的答案

---

import java.util.Arrays;

/**

* Definition for binary tree

* public class TreeNode {

*     int val;

*     TreeNode left;

*     TreeNode right;

*     TreeNode(int x) { val = x; }

* }

*/

public class Solution {

    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {

        TreeNode treeNode = new TreeNode(pre[0]); //每一步都先去确定根节点

        int i=0 ;       //找到根节点在中序遍历中的位置。

        for(int j=0;j<in.length;j++){

            if(in[j]==pre[0]){

                i=j;

                break;

            }

        }

        int [] leftPre = Arrays.copyOfRange(pre,1,i+1);//获得左子树的先序遍历

        int [] leftIn = Arrays.copyOfRange(in,0,i);//获得左子树的中序遍历

        int [] rightPre = Arrays.copyOfRange(pre,i+1,pre.length);//获得右子树的先序遍历

        int [] rightIn = Arrays.copyOfRange(in,i+1,in.length);//获得右子树的中序遍历

        if(leftIn.length!=0 || leftPre.length!= 0 ){//如果左子树不空，就递归求左子树

            treeNode.left = reConstructBinaryTree(leftPre,leftIn);

        }

        if(rightIn.length != 0 ||rightPre.length!= 0){//如果右子树不空，就递归求右子树

            treeNode.right = reConstructBinaryTree(rightPre,rightIn);

        }

        return treeNode;//每一次递归都会返回，但是只有最后一个返回起作用。（这是个问题）

    }

}
