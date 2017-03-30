---
title: Android productFlavor 去除不需要的依赖
date: 2017-03-10 22:23:00
categories:
- Program
- Android
tags: 
- Android
- Program
---

今天有个任务，处理一下项目的多渠道，这一块之前也没做过

其中有个知识点查了好久也没找到，可能是关键词没找对吧 =.=

虽然其中大部分东西都能从网上搜到，但还是把它写出来好了，当笔记了

<!-- more -->

主工程依赖了一个模块，叫它 lib 好了

然后 lib 是个工具库，里面接了很多第三方的库，各种支付什么的，然后为了以后方便，为这个库加个多渠道这样

先修改 lib 库的 build.gradle，添加 

```
android {
    publishNonDefault true
    productFlavors {
        channel1 {
        }
        channel2 {
        }
    }
}
```
然后再修改主工程的 build.gradle，网上搜到的基本都是这么做的

```
android {
    productFlavors {
        channel1 {
        }
        channel2 {
        }
    }
}
configurations {
    channel1ReleaseCompile
    channel1DebugCompile
    channel2ReleaseCompile
    channel2DebugCompile
}

dependencies {
    channel1ReleaseCompile project(path: ":lib", configuration: 'channel1Release')
    channel1DebugCompile project(path: ":lib", configuration: 'channel1Debug')
    channel2ReleaseCompile project(path: ":lib", configuration: 'channel2Release')
    channel2DebugCompile project(path: ":lib", configuration: 'channel2Debug')
}
```
这样没问题，但是试了下，应该可以有另外一种方法，个人感觉方便点

主工程 build.gradle 不改，只在 lib 的 build.gradle 里加一句 

```
android {
    defaultPublishConfig 'channel1Debug'
}
```

不过这样如果就没有对主工程进行一些配置了就是

之后对某个渠道增加一些特殊依赖并移除一些依赖

加依赖很简单

```
channel1Compile '...' // 就给 channel1 单独加了个依赖了
```

但现在如果大多数渠道都需要一个库，而少数渠道不需要，虽然也可以通过一个个单独添加的方式，但是这样就显得太麻烦了

就这个没能在网上找到，之后是对着文档一个类似的知识点琢磨出来的

```
configurations {
    channel2Compile.exclude group: 'com.example.lib'
}
```

这样就可以了



