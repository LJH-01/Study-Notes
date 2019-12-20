# 剑指offer第7题-斐波那契数列

## 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。

n<=39

---

public class Solution {

    public int Fibonacci(int n) {

        int [] f= {0,1,1};

        int f1=1,f2=1,fn=0;

        if(n<3){

            if(n>=0) fn=f[n];

        }

        else{

            for(int i=1;i<=n-2;i++){

                fn= f1+f2;

                f1=f2;

                f2=fn;

            }

        }

        return fn;

    }

}
