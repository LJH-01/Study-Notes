# 剑指offer第38题-二叉树的深度

求一棵二叉树的深度：

---

---

别人家的代码可真是太好了。

---

public class Solution {

    public int TreeDepth(TreeNode root) {

        if(root == null){

            return 0;

        }

        int left = TreeDepth(root.left);

        int right = TreeDepth(root.right);

        return Math.max(left, right) + 1;

    }

}

按照以往的层序遍历的套路也可以写出来，不过要加一些技巧，参考别人的写出

import java.util.LinkedList;

public class Solution {

    public int TreeDepth(TreeNode root) {

        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();

        if(root == null) return 0;

        queue.offer(root);

        int count = 0, depth = 0, nextcount = 1;

        while(!queue.isEmpty()){

            TreeNode tem = queue.poll();

            count++;

            if(tem.left!=null){

                queue.offer(tem.left);

            }

            if(tem.right!=null){

                queue.offer(tem.right);

            }

            if(count==nextcount){

                count = 0 ;

                nextcount = queue.size();

                depth++;

            }

        }

        return depth;

    }

}
