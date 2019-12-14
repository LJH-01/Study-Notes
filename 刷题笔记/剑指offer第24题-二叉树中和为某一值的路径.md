# 剑指offer第24题-二叉树中和为某一值的路径

## 题目描述

输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

---

二叉树的深度优先遍历

简洁但忘记排序的版本：

public class Solution {

    private ArrayList<ArrayList<Integer>> listAll = newArrayList<ArrayList<Integer>>();

    private ArrayList<Integer> list = new ArrayList<Integer>();

    //两个Arraylist 作为返回值。放在方法外面，做全局变量

    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {

        if(root == null) return listAll;//递归退出条件

    //本次递归要做的事情

        list.add(root.val);

        target -= root.val;

        if(target == 0 && root.left == null && root.right == null)

            listAll.add(new ArrayList<Integer>(list));

    //递归调用

        FindPath(root.left, target);

        FindPath(root.right, target);

    //因为本题中的特殊性，需要整个路径，因此回退操作。

        list.remove(list.size()-1);

        return listAll;

    }

}

带有排序的写法：

public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {

        ArrayList<ArrayList<Integer>> res=new ArrayList<>();

        ArrayList<Integer> cur=new ArrayList<>();

        helper(root,target,cur,res);

        Collections.sort(res, new Comparator<ArrayList<Integer>>() {//将res排序，，，非常精彩之处

            @Override

            public int compare(ArrayList<Integer> o1, ArrayList<Integer> o2) {

                if (o1.size()<o2.size()){

                    return 1;

                }else return -1;

            }

        });

        return res;

    }

    public void helper(TreeNode root,int target,ArrayList<Integer> cur,ArrayList<ArrayList<Integer>> res){

        if (root==null) return;

        int value=root.val;

        cur.add(value);

        if (target==value&&root.left==null&&root.right==null){

            res.add(new ArrayList<>(cur));

        }else {

            helper(root.left,target-value,cur,res);

            helper(root.right,target-value,cur,res);

        }

        cur.remove(cur.size()-1);

    }
