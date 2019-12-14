# 剑指offer第22题-按层打印二叉树

按层打印二叉树---广度优先搜索的典型例题，即把一层的先放到队列里，然后在处理队列。

收获：队列在Java中用Linkedlist

往队列尾添加：E offer（E e);

往队列头添加：E offerFirst (E e)

队列头出队列：E poll(E e)

队列尾出队列：E pollLast(E e)

详见 LinkedList API 

标准的代码

public class Solution {

    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {

        ArrayList<Integer> list = new ArrayList<Integer>();

        if(root==null){

            return list;

        }

        Queue<TreeNode> queue = new LinkedList<TreeNode>();

        queue.offer(root);

        while (!queue.isEmpty()) {

            TreeNode treeNode = queue.poll();

            if (treeNode.left != null) {

                queue.offer(treeNode.left);

            }

            if (treeNode.right != null) {

                queue.offer(treeNode.right);

            }

            list.add(treeNode.val);

        }

        return list;

    }

}

还有更高明点的，连队列也不用，实际上思路一模一样，只是使用ArrayList模仿一个队列

/**

思路是用arraylist模拟一个队列来存储相应的TreeNode

*/

public class Solution {

    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {

        ArrayList<Integer> list = new ArrayList<>();

        ArrayList<TreeNode> queue = new ArrayList<>();

        if (root == null) {

            return list;

        }

        queue.add(root);

        while (queue.size() != 0) {

            TreeNode temp = queue.remove(0);

            if (temp.left != null){

                queue.add(temp.left);

            }

            if (temp.right != null) {

                queue.add(temp.right);

            }

            list.add(temp.val);

        }

        return list;

    }

}
