# 剑指offer第44题-反转单词顺序

## 题目描述

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

---

投机取巧，

public class Solution {

    public String ReverseSentence(String str) {

        if(str.trim().length() == 0) return str;

        String [] strArray = str.split(" ");

        StringBuffer sb = new StringBuffer();

        for(int i=strArray.length-1;i>=0;i--){

            if(i==0){

                sb.append(strArray[i]);

            }else {

                sb.append(strArray[i]);

                sb.append(" ");

            }

        }

        return sb.toString();

    }

}

据说这道题的真正意图是  先将每个单词内部反转，在将整个字符串反转。
