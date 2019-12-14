# 剑指offer第34题-第一个只出现一次的字符

## 题目描述

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

---

别人家的代码就是好呀

import java.util.HashMap;

public class Solution {

   HashMap<Character, Integer> map = new HashMap<>();

    public int FirstNotRepeatingChar(String str) {

        if (str==null)return -1;

        int length = str.length();

        for(int i = 0;i<length;i++) {

            if(map.containsKey(str.charAt(i))){

                int value = map.get(str.charAt(i));

                map.put(str.charAt(i),value+1);

            }else{

                map.put(str.charAt(i),1);

            }

        }

     for(int i = 0;i<length;i++){

         if(map.get(str.charAt(i))==1)

             return i;

        }

        return -1;  

    }

}
