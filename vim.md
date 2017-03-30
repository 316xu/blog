---
title: 你可能不知道的 Vim 技巧
date: 2017-02-03 22:23:00
categories:
- Tool
- Editor
tags: 
- Vim
- Editor
- Tool
---

![](https://i1.wp.com/wp.laravel-news.com/wp-content/uploads/2016/09/vim8.png?resize=2200%2C1125)

> 之前又看到一篇文章，[Vim最少必要知识](http://www.jianshu.com/p/881a168d454a)，学到了不少东西，正好最近 Vim 用的比较多，写一些很有用但个人觉得比较偏的东西吧

<!-- more -->

## 批量注释与解注释

注释：  
0 移至行首
Ctrl + v 进入可视模式  纵向选取需要注释掉的行  
Shift + I 进入插入模式  
输入注释符  
Esc 完成注释

解注释：  
可视化模式下选取然后删除即可


## 格式化文本  
简单来说就是  = + 位移命令 然后这个范围内的都会格式化了  
这样 gg=G 就是格式化整个文件

## 滚动Ctrl  
Ctrl + y  
Ctrl + e

## 改变窗口大小
:vertical resize+10  
:resize-10

## 打开文件  
大家可能和我一样用的 ctrlp 这种插件来打开文件的，但如果想打开一个不在当前目录下的文件怎么办
使用 find 命令，指定路径打开
:find ~/main.js

暂时就这几个吧，以后想到了再加
