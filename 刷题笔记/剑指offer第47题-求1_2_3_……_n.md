# 剑指offer第47题-求1+2+3+……+n

## 题目描述

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

hel

即不能用乘除，循环，分支，计算1+2+3+……+n;

不能用判断语句，可以用短路运算符，不能用循环，用递归。

public class Solution {

    public int Sum_Solution(int n) {

        int sum = n;

        boolean flag = (sum>0)&&((sum+=Sum_Solution(--n))>0);

        return sum;

    }

}
