# 剑指offer第23题-判断序列是否是搜索二叉树的后续遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

二叉搜索树： 二叉查找树（Binary Search Tree），（又：二叉搜索树，二叉排序树）它或者是一棵空树，或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉排序树。

收获：

递归函数的一般写法：

第一步：递归退出判断条件

第二步：每一步递归需要做的工作，需要的准备工作

第三步：递归调用自身，如果是树，每一次递归有两路分支，可以  

return   recursive（右子树）&& recursive（左子树）

public class Solution {

    public boolean VerifySquenceOfBST(int [] sequence) {

        if(sequence.length==0)

            return false;

        if(sequence.length==1)

            return true;

        return ju(sequence, 0, sequence.length-1);

    }

    public boolean ju(int[] a,int star,int root){

        if(star>=root)

            return true;

        int i = root;

        //从后面开始找

        while(i>star&&a[i-1]>a[root])

            i--;//找到比根小的坐标

        //从前面开始找 star到i-1应该比根小

        for(int j = star;j<i-1;j++)

            if(a[j]>a[root])

                return false;

        return ju(a,star,i-1)&&ju(a, i, root-1);

    }

}
