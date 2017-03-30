---
title: 双系统下 Ubuntu 扩容
date: 2017-02-15 22:23:00
categories:
- Linux
tags: 
- Linux
description: 大概是我能找到的最简单的扩容方法了，最主要的是 0 风险，适合我这懒人
---
又想研究下 aosp，然后问题来了，Ubuntu 的空间不够了，那就从 Windows 里分一点过来好了，说干就干。

## 1. 从 Windows 下分出空间

进入 Windows，使用磁盘工具弄一个新的空间出来，这没啥可说的，但是再重启进 Ubuntu 出问题了

出现

```
Error: File '/boot/grub/i386-pc/normal.mod' not found
```





## 2. 修复 grub

只能想办法解决了，解决方案主要是参考[这篇文章](http://m.blog.csdn.net/article/details?id=51167830)，但是稍有不同，还是记录下来吧。

先使用 `ls` 命令，会出现诸如 `(hd0,msdos1)` 这样的，对应分区。

然后再不断的用 `ls (hd0,msdos1)/grub` 这条命令探测，直到找到为止，这里有一点需要注意，这个模式下是空格敏感的，也就是说逗号后面不能加空格。记录下所在分区，我的是 `(hd0, msdos8)`，然后

```
grub rescue>set root=(hd0,msdos8)
grub rescue>set prefix=(hd0,msdos8)/grub
grub rescue>insmod /grub/normal.mod
grub rescue>normal
```

这样就好了，进入系统后再执行 `sudo   update-grub   sudo   grub-install   /dev/sda` 修复 grub。



## 3. 创建新分区并挂载

使用 Ubuntu 下的 disk 工具，毕竟 fdisk 命令不太会用。。。

将刚才那个分出来的区域格式化一下就好，然后记录下它的分区号，我的是 `/dev/sdb7`

然后 ` sudo vim /dev/fstab` ，加一句

```
/dev/sdb7    /data   ext4    defaults    0    1
```

然后重启



## 4. 建立软连接

应该是可以将新的分区和原来的分区合并，但是这么做麻烦而且有风险，所以我选择了软连接这种方式，能用就好

```
sudo ln -s /data ~
```

这样在 /home 下就会有一个 data 的链接了


