# 剑指offer第33题-丑数

## 题目描述

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

---

别人家的代码就是好！！

该思路： 我们只用比较3个数：用于乘2的最小的数、用于乘3的最小的数，用于乘5的最小的

import java.util.ArrayList;

public class Solution {

    public int GetUglyNumber_Solution(int index) {

        if(index<=0) return 0;

        ArrayList<Integer> list = new ArrayList<Integer>();

        int i1=0,i2=0,i3=0;

        list.add(1);

        while(list.size()<index){

            int m1 = list.get(i1)*2;

            int m2 = list.get(i2)*3;

            int m3 = list.get(i3)*5;

            int min = Math.min(m1,Math.min(m2,m3));

            if(min == m1) i1++;

            if(min == m2) i2++;

            if(min == m3) i3++;

            list.add(min);

        }

        return list.get(list.size()-1);

    }

}
