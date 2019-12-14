# 剑指offer第41题-和为S的连续正整数序列

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

---

参考别人思路

import java.util.ArrayList;

public class Solution {

    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {

        ArrayList<ArrayList<Integer>>  result =  new ArrayList<>();

        int plow =1 , phigh =2;

        while(plow<phigh){

            int cur = (plow+phigh)*(phigh-plow+1)/2;//这是等差数列求和公式，不可能出现小数

            if(cur == sum){

                ArrayList<Integer> list = new ArrayList<Integer>();

                for(int i=plow;i<=phigh;i++){

                    list.add(i);

                }

                result.add(list);

                phigh ++;    **///这里写plow++也一样；**

            }

            else if(cur > sum){

                plow ++;

            }

            else if(cur < sum){

                phigh++;

            }

        }

        return result;

    }

}
