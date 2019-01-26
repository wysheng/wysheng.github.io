---
layout: post
title:  "基于内容的推荐 java实现"
date:   2018-7-11 11:45:38
categories: 数据挖掘
tags:  数据挖掘 推荐算法 java
---

* content
{:toc}
这是本人在cousera上学习机器学习的笔记，不能保证其正确性，谨慎参考
看完这一课后Content Based Recommendations 后自己用java实现了一下
1、下图是待处理的数据，代码使用数据和下图一样： 
![](https://img-blog.csdn.net/20170309164640433?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdHpoNDc2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
2、思路：对每个用户假定其为一个3维向量（在代码中初始化为[1,1,1]的转置，然后采用梯度下降法不断的对这个3维向量的值进行更新），假设更新到最后的向量值为[0，5，0]的转置，然后使用该向量和电影“Cute puppoes of love”的特征向量进行计算，即可得到该电影的预测分为4.95。 
![](https://img-blog.csdn.net/20170309165741276?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdHpoNDc2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
3、使用梯度下降法对某个用户的向量进行更新（我在代码中没有考虑正则化这一问题，现在还不懂正则化，后面学会了就附上加了正则化的）： 
![](https://img-blog.csdn.net/20170309170547804?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdHpoNDc2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
下图为没有使用正则化的函数：
![](https://img-blog.csdn.net/20170309170756480?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdHpoNDc2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

4、下面仅针对用户carol进行了代码实现
```
public class ContentBase {
    private static int[][] rate_set = { { 5, 5, 0, 0 }, { 5, -1, -1, 0 },
            { -1, 4, 0, -1 }, { 0, 0, 5, 4 }, { 0, 0, 5, -1 } };
    private static double[][] m_feature = { { 0.9, 0 }, { 1.0, 0.01 },
            { 0.99, 0 }, { 0.1, 1.0 }, { 0, 0.9 } };
    //仅针对用户carol进行了代码实现
    public static void main(String[] args) {
        double t = 0.1;
        double[] para = { 1.0, 1.0, 1.0 };
        double[] partial = new double[3];
        double min = 0.0;

        int i = 0, j, u,times=0;
        double temp,temp2;
        //100为用户2的向量学习次数
        while(times++<100){
            min=0.0;
            i=0;
            //该while循环计算代价函数
            while (i < 5) {
                temp = 0.0;
                if (rate_set[i][2] != -1) {
                    for (u = 0; u < 3; u++) {
                        if (u == 0)
                            temp += para[u];
                        else
                            temp += para[u] * m_feature[i][u - 1];
                    }
                    min += (temp - rate_set[i][2]) * (temp - rate_set[i][2]);
                }
                i++;
            }
            System.out.print("当用户 carol的向量值为[");
            for(j=0;j<3;j++)
                if(j!=2)
                    System.out.print(para[j]+",");
                else
                    System.out.println(para[j]+"]时，min="+min);
            System.out.println();

            for (j = 0; j < 3; j++) {
                i = 0;
                partial[j] = 0;
                while (i < 5) {
                    temp = 0.0;temp2=0.0;
                    if (rate_set[i][2] != -1) {
                        for (u = 0; u < 3; u++) {
                            if (u == 0)
                                temp += para[u];
                            else
                                temp += para[u] * m_feature[i][u - 1];
                        }
                        temp2 += temp - rate_set[i][2];
                        if (j != 0)
                            temp2 *= m_feature[i][j - 1];
                        partial[j]+=temp2;
                    }
                    i++;
                }
            }
            //根据求得的偏导数 partial来更新某用户的参数值
            for (j = 0; j < 3; j++) {
                para[j] = para[j] - t * partial[j];

            }
        }
    }
}
```
4、运行结果： 
![](https://img-blog.csdn.net/20170309172356566?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdHpoNDc2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)