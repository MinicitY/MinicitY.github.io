---
layout: post
title:  "LeetCode初级算法-数组（下）"
date:   2018-07-11
author: MinicitY
categories: LeetCode
tags: 数组
---

* content
{:toc}

## **加一**
>Given a non-empty array of digits representing a non-negative integer, plus one to the integer.
>
>The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.
>
>You may assume the integer does not contain any leading zero, except the number 0 itself.
>
>Example 1:
>
>Input: [1,2,3]
>Output: [1,2,4]
>Explanation: The array represents the integer 123.

几个需要处理的情形：初始值是0；遇到9需要进位；遇到连续的9需要连续进位；最高位不是9要在最高位停止进位；最高位是9需要初始化新数组以储存新数字10^k。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        int[] tempDigits;
        int i = digits.length - 1;//i = 位数
        if(digits[0] == 0)//单独判断0
           digits[0] = 1;
        else if(digits[i] == 9){//判断i位置的数是否需要进位
            for(; i >= 0 && digits[i] == 9; i--)
            digits[i] = 0;//进位处理后位
            if(i >= 0){//由于循环后还会执行一次i--，i >= 0即代表最高位没变成0，也就是最高位不是9
            digits[i]++;//进位处理前位
            return digits;
            }
            else {//最高位进位的情况
            tempDigits = new int[digits.length + 1];
            tempDigits[0] = 1;
            for(int j = 1; j < tempDigits.length; j++)
                tempDigits[j] = digits[j - 1];
            return tempDigits;
        }
    }
        else
            digits[i]++;
    return digits;
    
    }
}
```

## **有效的数独**

>Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
>
>    Each row must contain the digits 1-9 without repetition.
>    Each column must contain the digits 1-9 without repetition.
>    Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.
>
>The Sudoku board could be partially filled, where empty cells are filled with the character '.'.
>
>Note:
>
>    A Sudoku board (partially filled) could be valid but is not necessarily solvable.
>    Only the filled cells need to be validated according to the mentioned rules.
>    The given board contain only digits 1-9 and the character '.'.
>    The given board size is always 9x9.

同时检查行、列、块，并利用HashSet来线性查找重复值。

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int s;
        char[] nums = new char[9];
        for(int i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++)
                nums[j] = board[i][j];
            if(!judge(nums))
                return false;
            //检查行
            for(int k = 0; k < 9; k++)
                nums[k] = board[k][i];
            if(!judge(nums))
                return false;
            //检查列
            for(int r = 0; r < 9; r++){
                if(i%3 == 0 && r%3 == 0){
                    s = 0;
                    for(int p = i; p < i + 3; p++){
                        for(int q = r; q < r + 3; q++){
                            nums[s] = board[p][q];
                            s++;
                        }
                    }
                    if(!judge(nums))
                        return false;
                }
            }
            //检查块
        }
        return true;
    }
    
    public boolean judge(char[] nums){
     HashSet<Character> newSet = new HashSet<>();
        	for(int i = 0; i < nums.length; i++){
                if(nums[i] != '.'){
        		if(!newSet.contains(nums[i])){
        			newSet.add(nums[i]);
        		}   
                else
                    return false;
        }
    }
        return true;
}
}
```
## **旋转图像**

>You are given an n x n 2D matrix representing an image.
>
>Rotate the image by 90 degrees (clockwise).
>
>Note:
>
>You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.
>
>Example 1:
>
>Given input matrix = 
>
>[1,2,3]
  [4,5,6]
  [7,8,9]
>
>rotate the input matrix in-place such that it >becomes:
>
>[7,4,1]
>[8,5,2]
>[9,6,3]

先定义index为矩阵长度n再减1，也就是矩阵下标的最大值，即matrix\[index\]\[index\]刚好是最右下角。

画图之后，发现对于一个坐标为 **(row,column)** 的元素，顺时针旋转90°将变成 **(column,index-row)** ，再旋转变成 **(index-row,index-column)** ，再转变成 **(index-column,row)**。这四个坐标连线垂直，并且几何中心为 **(index/2,index/2)**。如果第一个位置的元素和第二个的交换，第一个再与第三个交换的，第一个再于第四个的交换，那么一、二、三、四位置的元素构成的相对位置组合正好顺时针旋转90°。下面代码块里的rotateElement方法就执行了这样的操作。

对于二维空间而言，调用一个同时控制四个对称地方的方法，我们只要专注坐标系的第一象限（以 **(index/2,index/2)** 为原点）。复杂度应该是O(n^2)。注意矩阵长度奇偶不同有差别。

```java
class Solution {
    int temp;
    int len;//矩阵长度
    int halfLen;//处理长度的奇偶差异
    public void rotate(int[][] matrix) {
        len = matrix.length;
        if(len%2 == 0)
            halfLen = len/2;
        else
            halfLen = len/2 + 1;
        for(int row = 0; row < halfLen; row++){
            for(int column = 0; column < len/2; column++){//处理第一象限
                rotateElement(row,column,matrix);//旋转与这个点相关的一共四个相对位置的组合点
            }
        }
    }
    //(row,column),(column,index-row),(index-row,index-column),(index-column,row)为这四个相对位置的组合点
    public void rotateElement(int row, int column, int[][]matrix){
        int index = matrix.length - 1;
        temp = matrix[column][index-row];
        matrix[column][index-row] = matrix[row][column];
        matrix[row][column] = temp;
        temp = matrix[index-row][index-column];
        matrix[index-row][index-column] = matrix[row][column];
        matrix[row][column] = temp;
        temp = matrix[index-column][row];
        matrix[index-column][row] = matrix[row][column];
        matrix[row][column] = temp;
    }
}
```