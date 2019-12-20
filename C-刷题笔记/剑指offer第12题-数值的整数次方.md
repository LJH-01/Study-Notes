# 剑指offer第12题-数值的整数次方

## 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

---

基本解法：

---

public class Solution {

    public double Power(double base, int exponent) {

        double product = 1.0;

        if(exponent == 0) product = 1.0;

        else if(exponent>0){

            for(int e = exponent; e>0;e--){

                product = product *base;

            }

        }

        else {

            for(int e = -1*exponent;e>0;e--){

                product = product *base;

            }

            product = 1.0/product;

        }

        return product;

  }

}

---

快速幂 

---

public int fastPower(int a, int b) {

        int ans = 1;

        int base = a;

        while (b != 0) {

            if ((b & 1) != 0) { //如果当前的次幂数最后一位(二进制数)不为0的话，那么我们将当前权值加入到最后答案里面去

                ans = ans * base;

            }

            //权值增加

            base = base * base;

            b >>= 1;

        }

        return ans;

    }
