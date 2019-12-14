# 剑指offer第19题--顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

---

import java.util.ArrayList;

public class Solution {

    public ArrayList<Integer> printMatrix(int [][] matrix) {

       ArrayList array =  new ArrayList<Integer>();

        int Hstart = 0;

        int Vstart = 0;

        int Hend = matrix.length-1;

        int Vend = matrix[0].length-1;

        //获取行列的起始位置

        while(Hstart <= Hend  && Vstart <= Vend){

            if(Vstart > Vend || Hstart > Hend) break;

            for(int j=Vstart;j<=Vend;j++){

                array.add(matrix[Hstart][j]);

            }

            Hstart+=1;

            if(Vstart > Vend || Hstart > Hend) break;

            for(int i=Hstart;i<=Hend;i++){

                array.add(matrix[i][Vend]);

            }

            Vend -=1;

            if(Vstart > Vend || Hstart > Hend) break;

            for(int j=Vend;j>=Vstart;j--){

                array.add(matrix[Hend][j]);

            }

            Hend -=1;

            if(Vstart > Vend || Hstart > Hend) break;

            for(int i=Hend;i>=Hstart;i--){

                array.add(matrix[i][Vstart]);

            }

            Vstart +=1;

        }

        return array;

    }

}

---

可运行版：

/**

*

*/

package tqr;

/**

* @author TQR

* 2019年8月23日

*/

import java.util.ArrayList;

import java.util.Iterator;

public class Solution2 {

    public static void main(String []args) {

        int[][] matrix = {{1,2,3,4},{5,6,7,8},{9,10,11,12},{13,14,15,16}};

        ArrayList<Integer> array=new ArrayList<Integer>();

        array = printMatrix(matrix);

        Iterator<Integer> it = array.iterator();

        for(Integer num:array) {

            System.out.print(num+" ");

        }

    }

    public static ArrayList<Integer> printMatrix(int [][] matrix) {

       ArrayList<Integer> array =  new ArrayList<Integer>();

        int Hstart = 0;

        int Vstart = 0;

        int Hend = matrix.length-1;

        int Vend = matrix[0].length-1;

        //获取行列的起始位置

        while(Hstart <= Hend  && Vstart <= Vend){

            if(Vstart > Vend || Hstart > Hend) break;

            for(int j=Vstart;j<=Vend;j++){

                System.out.println(matrix[Hstart][j]+" "+j);

                array.add(matrix[Hstart][j]);

            }

            Hstart+=1;

            System.out.println("Hstart="+Hstart);

            if(Vstart > Vend || Hstart > Hend) break;

            for(int i=Hstart;i<=Hend;i++){

                array.add(matrix[i][Vend]);

                System.out.println(matrix[i][Vend]+" "+i);

            }

            Vend -=1;

            System.out.println("Vend="+Vend);

            if(Vstart > Vend || Hstart > Hend) break;

            for(int j=Vend;j>=Vstart;j--){

                array.add(matrix[Hend][j]);

                System.out.println(matrix[Hend][j]+" "+j);

            }

            Hend -=1;

            System.out.println("Hend="+Hend);

            if(Vstart > Vend || Hstart > Hend) break;

            for(int i=Hend;i>=Hstart;i--){

                array.add(matrix[i][Vstart]);

                System.out.println(matrix[i][Vstart]+" "+i);

            }

            Vstart +=1;

            System.out.println("Vstart="+Vstart);

        }

        return array;

    }

}
