# 剑指offer第51题-构建乘积数组

## 题目描述

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

---

核心就是为了减少乘法运算次数

当成矩阵，先算下三角，再算上三角，这样不重复计算，相当于做了 2n次乘法。

import java.util.ArrayList;

public class Solution {

    public int[] multiply(int[] A) {

        int [] B = new int[A.length];

        for(int i=0;i<B.length;i++){

            B[i]= 1;

        }//initialization

        int tem1 = 1;

        for(int i=1;i<B.length;i++){

            tem1*=A[i-1];

            B[i]*=tem1;

        }

        int tem2=1;

        for(int i=B.length-2;i>=0;i--){

            tem2*=A[i+1];

            B[i]*=tem2;

        }

        return B;

    }

}
