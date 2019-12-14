# 剑指offer第35题-数组中的逆序对

## 题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

## 输入描述:

题目保证输入的数组中没有的相同的数字

数据范围：

对于%50的数据,size<=10^4

对于%75的数据,size<=10^5

对于%100的数据,size<=2*10^5

---

1.不能暴力遍历每一个数，时间复杂度是O（N2）。

2.此题实际上是考察归并排序，归并排序： 每次合并操作的平均时间复杂度为O(n)，而完全二叉树的深度为|log2n|。总的平均时间复杂度为O(nlogn)。而且，归并排序的最好，最坏，平均时间复杂度均为O(nlogn)。

归并排序是稳定排序，它也是一种十分高效的排序，能利用完全二叉树特性的排序一般性能都不会太差。java中Arrays.sort()采用了一种名为TimSort的排序算法，就是归并排序的优化版本。

发现：

这个过程也是一个递归过程，但是这个递归过程和以往的递归过程并不相同，

这个递归的机构是

1.递归退出条件

2.递归调用

3.从最后一次递归开始，倒着回去，每次需要做的事情。

以往的2，3 的位置互换，这样一来，本题的调用过程形成了一个上下两头小的罐子形状。

---

![归并排序.png](image/归并排序.png)

![归并排序2.png](image/归并排序2.png)

![归并排序3.png](image/归并排序3.png)

---

我的代码

public class Solution {

    public int InversePairs(int [] array) {

        int [] tem = new int [array.length];

        mergeSort(array,0,array.length-1,tem);

        return count;

    }

    private static int count = 0;

    private static void mergeSort(int [] array,int locbegin,int locend,int [] tem) {

        //当只有一个元素时退出，否则往下递归

        if(locbegin==locend) {

            return ;

        }

        //求中间那个数的下标，将原数组分而治之。

        int locmid = (locbegin+locend-1)/2;

        mergeSort(array,locbegin,locmid,tem);

        mergeSort(array,locmid+1,locend,tem);

        //当开始退栈的时候，这时候会面临从 locbegin 到 locend的两段

        int LL = locbegin;    //左侧开始的下标

        int LR = locmid;    //左侧结束的下标

        int RL = locmid+1;    //右侧开始的下标

        int RR = locend;    //右侧结束下标

        int k=locbegin;        //辅助数组，在本次范围内的开始下标

        //因为到这一步时，两边都是升序，只要有一边遍历到头，另一边就可以直接复制过去了。

        while(LL <= LR && RL<=RR) {

            if(array[LL] <= array[RL]) {    //左侧小与等于就取左边，这样可以保证是稳定排序

                tem[k] = array[LL];

                k++;

                LL++;

            }else {                            //否则是右侧小，只能取右侧。

                tem[k]=array[RL];

                count= (count+LR-LL+1)%1000000007;

                k++;

                RL++;

            }

        }

        //当有一侧遍历到头，根据上一步的循环控制条件可以看出，一定有另一侧没有遍历到头，需直接复制过来

        while(LL<= LR) {

            tem[k]=array[LL];

            k++;

            LL++;

        }

        while(RL<= RR) {

            tem[k]=array[RL];

            k++;

            RL++;

        }

        //这里要注意，tem只是一个辅助数组，如果在tem中排好序，却没有复制回array

        //那么退回到上一层递归，有拿着array操作时，会把此次排序结果覆盖掉，不许复制回原数组。

        for(int i=locbegin;i<=locend;i++) {

            array[i]=tem[i];

        }

    }

}
