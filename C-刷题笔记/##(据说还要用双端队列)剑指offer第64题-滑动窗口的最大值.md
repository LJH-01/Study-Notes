# ##(据说还要用双端队列)剑指offer第64题-滑动窗口的最大值

## 题目描述

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}

---

解析：

我的思路很容易，但不是最优解：n个数据 k的size 的时间复杂度是 （n-k）* k = kn- k^2;

import java.util.ArrayList;

public class Solution {

    public ArrayList<Integer> maxInWindows(int [] num, int size)

    {

        ArrayList list = new ArrayList<Integer>();

        if(size<=0) return list;

        int len = num.length;

        for(int i= 0 ; i<= len-size;i++){

            list.add(maxFrom(num,i,size));

        }

        return list;

    }

    private static int maxFrom(int [] num ,int start,int size){

        int max = num[start];

        for(int j=start+1;j<start+size;j++){

            if(num[j]>max) max = num[j];

        }

        return max;

    }

}
