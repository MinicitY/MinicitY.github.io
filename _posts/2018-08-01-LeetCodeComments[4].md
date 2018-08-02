---
layout: post
title:  "LeetCode初级算法-字符串（下）"
date:   2018-08-01
author: MinicitY
categories: LeetCode
tags: 字符串
---

* content
{:toc}

## **验证回文字符串**

>Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
>
>Note: For the purpose of this problem, we define empty string as valid palindrome.
>
>Example 1:
>
>Input: "A man, a plan, a canal: Panama"
>Output: true
>
>Example 2:
>
>Input: "race a car"
>Output: false

easy one. 字符串练手。




```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.length() == 0)
            return true;
        ArrayList<Character> letter = new ArrayList<>();
        for(char ch:s.toLowerCase().toCharArray()){
            if((ch >= 'a'&&ch <= 'z')||(ch >= 'A'&&ch <= 'Z')||(ch >= '0'&&ch <='9'))
                letter.add(ch);
        }
        int len = letter.size();
        for(int i = 0; i < len / 2; i++){
            if(letter.get(i) != letter.get(len - i -1))
                return false;
        }
        return true;
    }
}
```

## **数数并说**

>报数序列是指一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：
>
>1     1
>
>2     11
>
>3     21
>
>4     1211
>
>5     111221
>
>1 被读作  "one 1"  ("一个一") , 即 11。
>11 被读作 "two 1s" ("两个一"）, 即 21。
>21 被读作 "one 2",  "one 1" （"一个二" ,  "一个一") , 即 1211。
>
>给定一个正整数 n ，输出报数序列的第 n 项。
>
>注意：整数顺序将表示为一个字符串。
>
>示例 1:
>
>输入: 1
>输出: "1"
>
>示例 2:
>
>输入: 4
>输出: "1211"

首先需要理解题目。对字符串中连续相同的子字符串合并成「数量+值」，单个的则写成「"1"+值」。然后迭代进行，直到循环n-1次达到目标。

```java
class Solution {
	int pointer = 0;
    String tempResult = "";
    int amount = 0;
    
    public String countAndSay(int n) {
      String result = "1";
      if(n==1)
    	  return result;    
      while(n>1) { 
    	 while(pointer < result.length()-1) {//count next sequence
    		 analyze(result);//read out loud!
    	 }
    	 if(pointer==result.length()-1) {//case like 21 at end
    		 tempResult += "1";
    		 tempResult += result.charAt(result.length()-1);
    	 }
    	 //case like 21 || 11 at end
    	 result = tempResult;
    	 n--;
    	 pointer = 0;
    	 tempResult ="";
      }
      return result;
    }

    public void analyze(String result) {
    	 for(amount = 1;pointer <result.length()-1&&
    			 result.charAt(pointer)==result.charAt(pointer+1); ) {
		  	  amount++;
			  pointer++;
	  }
    	 
	  tempResult += Integer.toString(amount);
	  tempResult += result.charAt(pointer);
	  pointer++;
    }
}
```

## **最长公共前缀**

>Write a function to find the longest common prefix string amongst an array of strings.
>
>If there is no common prefix, return an empty string "".
>
>Example 1:
>
>Input: ["flower","flow","flight"]
>Output: "fl"
>
>Example 2:
>
>Input: ["dog","racecar","car"]
>Output: ""
>Explanation: There is no common prefix among the input strings.
>
>Note:
>
>All given inputs are in lowercase letters a-z.

要先计算最短字符串有多长，处理一些特例，然后再查找相同字符（感觉储存字符串数组，有点像储存二维数组啊）。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int size = strs.length;
        if(size==0)
            return "";
        if(size==1)
            return strs[0];
        else{
        int min = strs[0].length();
        for(int i = 0; i < size-1; i++){
            if(min > strs[i+1].length())
                min = strs[i+1].length();//know how short could a str be
        }
        if(min==0)
            return "";
        for(int k = 0; k < min; k++){
            for(int j = 0; j < size-1; j++){
                if(strs[j].charAt(k)!=strs[j+1].charAt(k)){
                    if(k==0)
                        return "";//no common prefix
                    j = size;//this is just a flag
                    return strs[0].substring(0,k);
                }
            }
        }
        return strs[0].substring(0,min);
        }
    }
}
```