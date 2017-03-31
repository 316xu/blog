---
title: 从零开始用命令行创建一个多模块 Android 工程
date: 2017-02-03 22:23:00
categories:
- Program
- Android
tags: 
- Program
- Gradle
- Android
---

![](https://image.xujifa.cn/model/nhtrkNx2iS3Ez2zQhdwzhsSz5yPyRayP)

闲来无事，突发奇想，以前写 Android 工程，都是先用 AS 创建一个工程，然后开始编写代码，创建工程的过程完全不用自己操心  

那么，能不能自己从零开始构建一个工程呢？
<!-- more -->


## 建立主工程

先建立一个新目录 `mkdir GradleDemo`


既然是 Gradle 工程，那首先肯定得在根目录下新建一个 build.gradle 文件啊，并添加上一下代码

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

运行 `gradle build`，虽然没什么输出，但至少没报错是吧。。。


接下来让我们来建立 app 目录 `mkdir app`，并在其下建立 build.gradle 文件

```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"
    defaultConfig {
        applicationId "cn.xujifa.gradledemo"
        minSdkVersion 23
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
}
```

现在让我们在 app/ 下运行 gradle build，错误发生了

```
A problem occurred evaluating root project 'app'.
> Plugin with id 'com.android.application' not found.

```

`com.android.application` 没有找到？仔细看看，我们在这个文件里连 repositories 都没定义了，当然找不到这个插件  

可是以前用 AS 创建的工程里也没有在这个文件里特别注明 repositories 啊

这个时候可以在根目录下的 build.gradle

```
allprojects {
    repositories {
        jcenter()
    }
}
```
当然，在 app/build.gradle 里加 repositories 也没问题

但是这样还不够，我们并没有将两个 build.gradle 联系起来

在根目录下新建 settings.gradle 

```
include ':app'
```

再次运行 gradle build，果然，刚才的错误没了，新的错误又来了

```
Cannot read packageName from /home/xujifa/data/workspace/android/GradleDemo/app/src/main/AndroidManifest.xml
```

看样子是时候开始正式加入 Android 代码了

在 app/ 下建立 src 文件夹，之后添加 Android 相关代码，目录结构下面这样，代码就不贴了，都是些大家熟悉的东西

```
src
    └── main
        ├── AndroidManifest.xml
        ├── java
        │   └── MainActivity.java
        └── res
            ├── layout
            │   └── activity_main.xml
            └── values
                ├── strings.xml
                └── styles.xml
```

再次运行 gradle build，你会发现已经可以编译出 apk 文件了，接下来在加上一个模块就算大功告成了

## 加入新模块

在根目录下新建文件夹 lib

同样的，在 lib/ 下建立 build.gradle，与 app/build.gradle 唯一的差别就是 plugin 是 `com.android.library`

```
apply plugin: 'com.android.library'

android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"

    defaultConfig {
        minSdkVersion 23
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"


    }
    buildTypes {
        release {
            minifyEnabled false
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
}
```

之后也是在 lib/ 下建立 src 目录

```
src
    └── main
        ├── AndroidManifest.xml
        ├── java
        │   └── cn
        │       └── xujifa
        │           └── lib
        │               └── Ppx.java
        └── res
            └── values
                └── strings.xml
```

最后呢，需要把这个模块加到主工程里

将 settings.gradle 改成

```
include ':app', ':lib'
```

然后在 app/build.gradle 中加上

```
compile project(':lib')
```


所有的到此就算完成了，还是蛮简单的吗，以前就只会用 AS，离了就不知道怎么办才好了，好歹算是有点长进。

只是做了一点微小的工作，还是顺便求波赞
