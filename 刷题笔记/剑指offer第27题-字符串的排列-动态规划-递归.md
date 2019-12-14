# 剑指offer第27题-字符串的排列-动态规划-递归

## 题目描述

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

## 输入描述:

输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

思路：

递归，动态规划

对每一层递归，遍历数组。假设有123453

第一层递归的遍历：固定1，求23453的全排列，

然后1，2 互换，固定2，求13453的全排列，完事儿换回来，还是123453；

然后1和 3互换，固定3 ，求12453的全排列，完事儿赶紧换回来；

……

当遇到最后一个3时，假设还互换，那就是，固定3，求23451的全排列，和上面重复，因此不在换。

递归退出条件：当数组只有一个字符时，本身就是全排列了，转为String添加到list。

每一步递归要做的事：

遍历字符数组，forEach：{

    将当前字符加到set里，

    交换字符，（因为是从自身开始的，循环的一次是自己和自己交换。）

    递归调用；

    交换字符；（恢复顺序继续往下遍历）

}

---

public ArrayList<String> Permutation(String str){

        ArrayList<String> list = new ArrayList<String>();

        if(str!=null && str.length()>0){

            PermutationHelper(str.toCharArray(),0,list);//字符数组，起始下标，list

            Collections.sort(list);

        }

        return list;

    }

    private void PermutationHelper(char[] chars,int i,ArrayList<String> list){

        if(i == chars.length-1){

            list.add(String.valueOf(chars));//递归退出条件，即递归到最后一个元素了。

        }else{

            Set<Character> charSet = new HashSet<Character>();

            for(int j=i;j<chars.length;++j){//遍历chars数组

                if(j==i || !charSet.contains(chars[j])){

//把第一个和后面与之不等的字符放进set里。

                    charSet.add(chars[j]);

                    swap(chars,i,j);

//本次递归要做的事情

//然后交换求全排列，开始压栈，即以

                    PermutationHelper(chars,i+1,list);//递归调用

 swap(chars,j,i);//为了使遍历继续往下进行

//恢复工作，为了不影响我的循环

                }

            }

        }

    }

    private void swap(char[] cs,int i,int j){

        char temp = cs[i];

        cs[i] = cs[j];

        cs[j] = temp;

    }

完整可执行版：我默写的:

学到

1.字符串全排列的求法

2.set的使用实现类是HashSet 

3.字符串转字符数组是：str.toCharArray();字符数组转字符串是 String.valueOf(char[])

package solution;

/**

* @author TQR

* 2019年8月28日

*/

import java.util.ArrayList;

import java.util.HashSet;

import java.util.Iterator;

import java.util.Collections;

public class Solution_Offer27_Permutation {

    public ArrayList<String> Permutation(String str) {

           ArrayList<String> list = new ArrayList<String>();

           if(str!=null && str.length()!=0){

               PermutationHelper(str.toCharArray(),0,list);

               Collections.sort(list);

           }

           return list;

    }

    public void PermutationHelper(char[] chars,int start,ArrayList<String> list) {

        if(start == chars.length-1) {

            list.add(String.valueOf(chars));

        }else {

            HashSet<Character> set = new HashSet<Character>();

            for(int i=start;i<chars.length;i++) {

                if(!set.contains(chars[i])) {

                    set.add(chars[i]);

                    swap(start,i,chars);

                    PermutationHelper(chars,start+1,list);

                    swap(start,i,chars);

                }

            }

        }

    }

    public void swap(int start,int i,char[] chars) {

        char temp = chars[i];

        chars[i] = chars[start];

        chars[start] = temp;

    }

    public static void main(String []args) {

        Solution_Offer27_Permutation s = new Solution_Offer27_Permutation();

        ArrayList<String> lis = new ArrayList<String>();

        lis = s.Permutation("alibaba");

        Iterator<String> it =  lis.iterator();

        int i=1;

        for(String tep:lis) {

            System.out.println("count "+i+" "+tep);

            i++;

        }

    }

}
