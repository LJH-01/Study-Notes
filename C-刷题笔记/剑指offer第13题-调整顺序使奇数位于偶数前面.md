# 剑指offer第13题-调整顺序使奇数位于偶数前面

## 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

---

这道题考察的是排序的算法，我给它做成了Queue的使用练习

---

我的代码

import java.util.Queue;

import java.util.LinkedList;

public class Solution {

    public void reOrderArray(int [] array) {

        Queue<Integer> q = new LinkedList<Integer>();

        for(int i=0;i<array.length;i++){

            if(array[i]%2==1){

                q.add(array[i]);

            }// odd enqueue

        }

        for(int i=0;i<array.length;i++){

            if(array[i]%2==0){

                q.add(array[i]);

            }// even enqueue

        }

        for(int i=0;i<array.length;i++){

            array[i]=q.poll();

        }

    }

}

---

别人的：

public class Solution {

    public void reOrderArray(int [] array) {

        //相对位置不变，稳定性

        //插入排序的思想

        int m = array.length;

        int k = 0;//记录已经摆好位置的奇数的个数

        for (int i = 0; i < m; i++) {

            if (array[i] % 2 == 1) {

                int j = i;

                while (j > k) {//j >= k+1

                    int tmp = array[j];

                    array[j] = array[j-1];

                    array[j-1] = tmp;

                    j--;

                }

                k++;

            }

        }

    }

}
