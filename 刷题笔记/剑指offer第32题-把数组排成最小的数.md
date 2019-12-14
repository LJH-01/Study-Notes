# 剑指offer第32题-把数组排成最小的数

## 题目描述

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

---

练习comparator的用法

Collections.sort(list,comparator)

Arrays.sort(T[],comparator)

package solution;

import java.util.ArrayList;

import java.util.Collections;

import java.util.Comparator;

/**

* @author TQR

* 2019年8月30日

*/

public class Solution_Offer32_StringSort {

    public static String PrintMinNumber(int [] numbers) {

        ArrayList<String> list = new ArrayList<String>();

        for(int i=0;i<numbers.length;i++) {

            list.add((String.valueOf(numbers[i])));

        }

Collections.sort(list,new Comparator<String>() {

            @Override

            public int compare(String arg0, String arg1) {

                // TODO Auto-generated method stub

                String str1=arg0+arg1;

                String str2=arg1+arg0;

                return str1.compareTo(str2);

            }

        });//关键在于此排序，如果是s1s2>s2s1,说明s1<s2;

        StringBuilder sb = new StringBuilder();

        for(String str:list) {

            //System.out.println(str);

            sb=sb.append(str);

        }

        return sb.toString();

    }

    public static void main(String []args) {

        int [] num = {3,32,321};

        System.out.println( PrintMinNumber(num));

    }

}
