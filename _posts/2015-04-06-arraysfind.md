---
layout: post
title:  "剑指offer：二维数组中的查找"
date:   2017-04-06 15:14:54
categories: 算法和数据结构
tags: 数据结构 Java 剑指offer 面试
---

* content
{:toc}

## 题目描述

题目来自 [剑指offer]   
在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
## 思路
选取右上角元素，
  元素大于Key--》剔除整列，
  元素小于key--》剔除整行*/
 
 
注意点就是循环的条件
对于row行 从上0开始递增，停止的条件是小于行的长度
对于column列，从右边列的长度开始递减，停止的条件是大于等于零
（因为查询最右下角的元素时，第0列是允许的不能停止。）
![avatar](https://img-blog.csdn.net/201807201033365?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4Mjc3MDMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Java代码如下：
```java
/**
 * 问题描述:
 *
 *   在一个二维数组中，每一行都按照从左到右递增的顺序排序，
 * 每一列都按照从上到下递增的顺序排序。请完成一个函数，
 * 输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
 *
 * 输入描述:
 * array: 待查找的二维数组
 * target: 查找的数字
 *
 * 输出描述:
 * 查找到返回true,查找不到返回false
 */
public class MatrixSearch {

    /**
     * 解决方法:
     * 将target与二维数组array第一行最后一个数做比较,如果target大于该行数,则不考虑第一行,相反不考虑最后一列,相等返回true
     *
     * @param array
     * @param target
     * @return
     */

    public static boolean find(int[][] array,int target){
        if (array==null || array.length<1 ||  array[0].length<1){
            return false;
        }
        int rows = array.length;
        int cols = array[1].length;
        int row = 0;
        int col = cols - 1;
        while (row<rows && col<cols && row>=0 && col>=0){
            if (array[row][col] == target){
                return true;
            }else if (array[row][col] > target){
                col--;
            }else {
                row++;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] array = {
                {1,2,8,9},
                {2,4,9,12},
                {4,7,10,13},
                {6,8,11,15}
        };
        System.out.println(find(array,22));
        System.out.println(find(array,12));
        System.out.println(find(null,10));
    }
}
```
---