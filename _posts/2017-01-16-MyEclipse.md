---
layout: post
title:  "Myeclipse 中 项目前有感叹号是什么意思"
date:   2017-1-16 10:45:38
categories: 开发工具
tags:  Myeclipse 软件开发工具
---

* content
{:toc}

## 说明你倒入的项目可能缺少引用的jar包，或者jar包的位置不对，在选定项目上
解决办法如下：
如图所示：点击那个Libraries，
![](https://gss0.baidu.com/-vo3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=721b1f63050828386858d41288a98539/1ad5ad6eddc451dad4afb897b4fd5266d0163274.jpg)

![](https://gss0.baidu.com/-fo3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=432f6491e51190ef01ae9ad9fe2bb12e/6a63f6246b600c33415e6ab4184c510fd9f9a1a3.jpg)
如果项目之前有!号的话，一般来说，下面的引入的jar包会有前面有小红叉的，你edit一下错误的，改正一下就可以了。

选择弹出对话框的“Libraries”

（此时项目中的jar肯定有错误路径引入，可以直接删除错误的引入），

   之后选择“add External JARs”；



第三步：找到jar文件的存放路径，选择一个或多个后，

直接重新引入即可，项目重新编译后，红色叹号会自动消失。


