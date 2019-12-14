# 剑指offer第40题-数组中只出现一次的数字

## 题目描述

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

---

---

这题可真是恶心哟，

看别人的代码吧，异或操作，^ 如果两个位上相等，异或为0，两个位上不等，异或为1.

先对所有数一起异或，，最后成对的数字异或为零，最终会剩下一个不为零的o1串，是两个不同数字异或的值。

找到这个o1串第一个1.即第一个不同的位，以此位将数组分成两个部分，两部分长度可能不相等，但是相同的数会被分到同一部分。

对两个数组，内部各自异或，则分别得到落单的数。

public class Solution {

    public void FindNumsAppearOnce(int[] array, int[] num1, int[] num2)    {

        int length = array.length;

        if(length == 2){

            num1[0] = array[0];

            num2[0] = array[1];

            return;

        }

        int bitResult = 0;

        for(int i = 0; i < length; ++i){

            bitResult ^= array[i];

        }

        int index = findFirst1(bitResult);

        for(int i = 0; i < length; ++i){

            if(isBit1(array[i], index)){

                num1[0] ^= array[i];

            }else{

                num2[0] ^= array[i];

            }

        }

    }

    private int findFirst1(int bitResult){

        int index = 0;

        while(((bitResult & 1) == 0) && index < 32){

            bitResult >>= 1;

            index++;

        }

        return index;

    }

    private boolean isBit1(int target, int index){

        return ((target >> index) & 1) == 1;

    }

}

---

---

上面是模块化的，我将其改为一个方法内的

//num1,num2分别为长度为1的数组。传出参数

//将num1[0],num2[0]设置为返回结果

public class Solution {

    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {

        int length  = array.length;

        int bit = 0;

        for (int i=0;i<length;i++){

            bit ^= array[i];

        }

        int index = 0;

        while((bit & 1) == 0){            **////两个异或操作很值得学习**

            bit = bit >> 1;

            index ++;

        }

        for(int i=0;i<length;i++){

            if(((array[i] >> index)&1) == 1){        **////两个异或操作很值得学习**

                num1[0] ^= array[i];

            }

            else num2[0] ^= array[i];

        }

    }

}
