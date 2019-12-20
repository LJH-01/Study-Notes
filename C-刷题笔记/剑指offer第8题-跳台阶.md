# 剑指offer第8题-跳台阶

## 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

---

public class Solution {

    public int JumpFloor(int target) {

        int f1=1,fn=0,f2=2;

        if(target==1) return f1;

        if(target==2) return f2;

        else{

            for(int i=1;i<=target-2;i++){

                fn= f1+f2;

                f1=f2;

                f2=fn;

            }

            return fn;

        }

    }

}

---

这个题关键在于分析，分析后发现是一个斐波那契数列。
