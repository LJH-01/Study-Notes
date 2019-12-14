# 剑指offer第53题-表示数值的字符串

## 题目描述

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

---

解析：考察编译原理，正则表达式引擎的简单实现。

我的方法主要就是分情况讨论。

看有没有e出现 ：

1.有e，

        1.1   e左侧是整数吗？  e右侧是无符号正整数或者负整数吗？

        1.2   e左侧是小数码？  e右侧是无符号正整数或者负整数吗？

2.五e，

        2.1   e是整数吗？

        2.2   e是小数吗？

---

关键就是三个判断方法  1，是整数吗？     2.是小数吗？     3.是无符号正整数或者负整数吗？

代码：（不想再看第二遍）

public class Solution {

    public boolean isNumeric(char[] str) {

        if(str.length==0) return false;

        for(int i=0;i<str.length;i++){

            if(str[i] == 'e' || str[i] == 'E'){

                if( isDecimal(str,0,i-1) && isUnsignedInteger(str,i+1,str.length-1))

                    return true;

                if( isInteger(str,0,i-1) && isUnsignedInteger(str,i+1,str.length-1))

                    return true;

            }

        }

        if( isInteger(str,0,str.length-1) ) return true;

        else if(isDecimal(str,0,str.length-1)) return true;

        return false;

    }

    private static boolean isInteger(char[] str,int begin,int end){

        if(end<begin) return false;

        for(int i=begin;i<=end;i++){

            if(str[i]<'0' || str[i]>'9'){

                if(i==begin) { if (str[i] == '+' || str[i] == '-') ;}

                else return false;

            }

        }

        return true;

    }

    private static boolean isUnsignedInteger(char[] str,int begin,int end){

        if(end<begin) return false;

        for(int i=begin;i<=end;i++){

            if(str[i]<'0' || str[i]>'9'){

                if(i==begin) { if (str[i] == '-') ;}

                else return false;

            }

        }

        return true;

    }

    private static boolean isDecimal(char[] str,int begin,int end){

        if(end<begin) return false;

        int i =begin;

        for (;i<=end;i++){

            if(str[i]=='.') break;

        }

        if( isInteger(str,begin,i-1) && isUnsignedInteger(str,i+1,end) )return true;

        return false;

    }

}
