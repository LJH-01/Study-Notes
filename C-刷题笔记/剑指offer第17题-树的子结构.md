# 剑指offer第17题-树的子结构

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

---

/**

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

    public boolean HasSubtree(TreeNode root1,TreeNode root2) {

        boolean result = false;   //先把标志设置成false，可以用短路运算。

            if(root1 != null && root2 != null){   //节点不空的时候往下开始

                if(root1.val == root2.val){      //1.根节点相同的情况下，要继续往下比对，此处写了另一个方法，很厉害，我在这里还想在HasSubtree方法里递归，这样不行，别人的处理方法就是另起了一个方法。

 result = DoesTree1HaveTree2(root1,root2);//精华

                }

                if(!result){result = HasSubtree(root1.left, root2);}    //根节点不同，去和左子树比对

                if(!result){result = HasSubtree(root1.right, root2);}   //左子树比对失败和右子树比对

            }

            return result;

    }

    public boolean DoesTree1HaveTree2(TreeNode root1,TreeNode root2){

            if(root2 == null) return true;

            if(root1 == null) return false;

            if(root1.val != root2.val) return false;

 return DoesTree1HaveTree2(root1.left, root2.left) && DoesTree1HaveTree2(root1.right, root2.right); //精华

        }

}
