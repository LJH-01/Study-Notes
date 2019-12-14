# 剑指offer第30题-连续子数组的最大和

例如:{6,-3,-2,7,-15,1,2,2}。 返回它的最大连续子序列的和。 (子向量的长度至少是1)

---

看过别人思路后做

因为和最大的子序列肯定是从正数开始加的，所以

假设 和最大的子序列大于0，那就当sum<0时 将sum置为0，max记录出现过的最大sum

    这样有一种例外，即全部都小于等于0；难么正确应该输出最大的那个负数，仍然用max记录即可解，决，即不算例外情况了。

---

/**

*

*/

package solution;

/**

* @author TQR

* 2019年8月29日

*/

public class Solution_Offer30_FindGreatestSumOfSubArray {

    /**

     * 利用了一个性质，即和最大的子序列一定是从整数开始，因此当sum<0时，sum置零

     * 特殊情况，全部小于零；那就输出最大的负数，也不算特殊情况。

     */

    public static int FindGreatestSumOfSubArray(int[] array) {

        int sum = 0;

        int max = array[0];

        boolean flag = false;

        for(int i=0;i<array.length;i++) {

            sum+=array[i];

            if(sum>0) {//和大于零，

                if(sum>max) max = sum;

            }

            else {

                sum=0;

                if(array[i]>max) max = array[i];

            }

        }

        return max;

    }

    public static void main(String []args) {

        int [] test = {6,-3,-2,7,-15,1,2,2};

        System.out.println(FindGreatestSumOfSubArray(test));

    }

}
