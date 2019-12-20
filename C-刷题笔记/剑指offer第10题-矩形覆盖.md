# 剑指offer第10题-矩形覆盖

## 题目描述

我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法

---

分析发现又一个斐波那契数列 f(n)= f(n-1)+f(n-2)

---

public class Solution {

    public int RectCover(int target) {

        int f1=1,f2=2,fn=0;

        if(target==1) return f1;

        if(target==2) return f2;

        else{

            for(int i=1;i<=target-2;i++){

                fn=f1+f2;

                f1=f2;

                f2=fn;

            }

            return fn;

        }

    }

}
