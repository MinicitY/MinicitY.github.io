---
layout: post
title:  "LeetCode初级算法-字符串（上）"
date:   2018-07-20
author: MinicitY
categories: LeetCode
tags: 字符串
---

* content
{:toc}

## **颠倒整数**

>Given a 32-bit signed integer, reverse digits of an integer.
>
>Example 1:
>
>Input: 123
>Output: 321
>Note:
>Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

利用取余数的性质。




```java
class Solution {
    public int reverse(int x){
        int remainder;
        int reverseX = 0;
        int temp = 0;
        while(x!=0){
            remainder = x % 10;
            temp = reverseX;
            reverseX *= 10;
            reverseX += remainder;
            x /= 10;
        }
        if(reverseX / 10 != temp)
            return 0;
        return reverseX;
    }
}
```

## **字符串中的第一个唯一字符**

> Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.
>
>Examples:
>
>s = "leetcode"
>return 0.
>
>s = "loveleetcode",
>return 2.
>
>Note: You may assume the string contain only lowercase letters. 

利用链表Map来储存数据。

```java
class Solution {
    public int firstUniqChar(String s) {
        Map<Character,Integer> tempMap = new LinkedHashMap<Character,Integer>();
        for(char c: s.toCharArray()){
        if(!tempMap.containsKey(c)){
            tempMap.put(c, 1);
        }else{
            tempMap.put(c, 2);
        }
        }
            for(Map.Entry<Character,Integer>flag:tempMap.entrySet()){
                if(flag.getValue() == 1)
                    return s.indexOf(flag.getKey());
            }
        return -1;
    }
}
```