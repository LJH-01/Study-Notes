# 剑指offer第57题-二叉树的下一个节点

## 题目描述

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

---

关键在于分情况讨论，这个总结过程是重点，只用总结的到位了，写代码很简单，要注意思考的过程

1.右子树不空---------》右子树的最左叶子结点

2.右子树空

  2.1 相对某个节点是在左子树上，--------------》父节点

  2.2 相对某个节点是在右子树上，--------------》找到一个这样的节点A，相对于A，本节点在左子树上。

注意2.2的越界

代码：

/*

public class TreeLinkNode {

    int val;

    TreeLinkNode left = null;

    TreeLinkNode right = null;

    TreeLinkNode next = null;

    TreeLinkNode(int val) {

        this.val = val;

    }

}

*/

public class Solution {

    public TreeLinkNode GetNext(TreeLinkNode pNode){

        if(pNode.right != null){

            //右子树不空

            TreeLinkNode cur = pNode.right;

            while(cur.left != null){

                cur = cur.left;

            }

            return cur;

        }

        else{//右子树是空的。

            if(pNode.next == null) return null ;//父节点，且没有右子树。结束。

            if(pNode.next.left == pNode){

                //在某个父节点的左子树上，且它自己的右子树为空。

                return pNode.next; //返回父节点

            }

            else {

                //在某个父节点的右子树上，且自身右子树为空。

                TreeLinkNode tem = pNode;

                TreeLinkNode cur = tem.next;

                while(! (cur.left == tem)){

                    if(cur.next!=null)cur = cur.next;

                    else return null;

                    tem = tem.next;

                }

                return cur;

            }

        }

    }

}
