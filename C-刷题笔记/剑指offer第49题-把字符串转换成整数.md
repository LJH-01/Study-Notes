# 剑指offer第49题-把字符串转换成整数

## 题目描述

将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

## 输入描述:

输入一个字符串,包括数字字母符号,可以为空

## 输出描述:

如果是合法的数值表达则返回该数字，否则返回0

示例1

## 输入

复制

+2147483647

1a33

## 输出

复制

2147483647

0

这道题可能考察的是考虑问题的完整性，包括符号位处理，整型溢出等多种边界要考虑好。

---

/**

*

*/

package solution;

/**

* @author TQR

* 2019年9月10日

*/

public class Solution_Offer49 {

    public static int StrToInt(String str) {

        char [] chr = str.toCharArray();

        int base = 1;

        int result = 0;

        for(int i=chr.length-1;i>=0;i--){

            //System.out.println(chr[i]>'9');

            if((chr[i]<'0' || chr[i]>'9') && i!=0)  return 0;       

            else if(i==0){

                if (chr[i]=='+') ;

                else if(chr[i]=='-') result = -result;

                else {result += (chr[i]-'0')*base; base*=10;}

            }else {

                result += (chr[i]-'0')*base;

                base*=10;

            }

        }

        return result;

    }

    public static void main (String []args) {

        String str = "aaaa";

        System.out.println(StrToInt(str));

    }

}
