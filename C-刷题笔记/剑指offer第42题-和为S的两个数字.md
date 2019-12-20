# 剑指offer第42题-和为S的两个数字

## 题目描述

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

## 输出描述:

对应每个测试案例，输出两个数，小的先输出。

同样设置头尾指针即可此题和41题类似，比41题更简单。

---

import java.util.ArrayList;

public class Solution {

    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {

        ArrayList<Integer> list = new ArrayList<Integer>();

        int len = array.length;

        int low = 0, high = len-1;

        while(low<high){

            if(array[low]+array[high] == sum){

                list.add(array[low]);

                list.add(array[high]);

                return list;

            }

            else if(array[low]+array[high]<sum){

                low++;

            }

            else if(array[low]+array[high]>sum){

                high--;

            }

        }

        return list;

    }

}
