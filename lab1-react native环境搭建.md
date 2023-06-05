# 安装依赖

## 下载jdk11

```
https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html
```

[(19条消息) 2020年最新 java JDK 11 下载、安装与环境变量配置教程_天天上网_c的博客-CSDN博客_jdk11](https://blog.csdn.net/qq_41873673/article/details/108027074?ops_request_misc=%7B%22request%5Fid%22%3A%22166332632916782414957238%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166332632916782414957238&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-108027074-null-null.142^v47^control,201^v3^control_1&utm_term=jdk11下载 &spm=1018.2226.3001.4187)



## 下载nodejs

由于我之前下载过node所以直接运行命令

```powershell
npx nrm use taobao
```

安装yarn

```
npm install -g yarn
```

 



# 下载Android studio

## 设置SDK

![image-20220918135654539](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918135654539.png)

选择右下角的show package details

![image-20220916104353588](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916104353588.png)



![image-20220918135727849](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918135727849.png)



![image-20220918135748735](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918135748735.png)



**下载sdk tools的时候这个选项可以选，是intel为了提高模拟机的使用流畅度的，但是因为我是锐龙的cpu所以下载之后会报错，这个要注意一下。**

![image-20220918135827489](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918135827489.png)





# 设置环境变量

React Native 需要通过环境变量来了解你的 Android SDK 装在什么路径，从而正常进行编译。

打开`控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量` -> `新建`，创建一个名为`ANDROID_SDK_ROOT`的环境变量（系统或用户变量均可），指向你的 Android SDK 所在的目录（具体的路径可能和下图不一致，请自行确认）

这里我设置在系统变量：

![image-20220916104708968](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916104708968.png)



变量值是sdk下载路径，可以通过android studio 的sdk mannager查看

![image-20220918141712717](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918141712717.png)





打开`控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量`，选中**Path**变量，然后点击**编辑**。点击**新建**然后把这些工具目录路径添加进去：platform-tools、emulator、tools、tools/bin



```
%ANDROID_SDK_ROOT%\platform-tools
%ANDROID_SDK_ROOT%\emulator
%ANDROID_SDK_ROOT%\tools
%ANDROID_SDK_ROOT%\tools\bin
```

![image-20220916104832073](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916104832073.png)





# 创建新项目

建立一个文件夹用于存放项目

建立新项目

```
npx react-native init AwesomeProject
```

![image-20220916105259262](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916105259262.png)





# 添加安卓设备

![image-20220918141820835](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918141820835.png)

![image-20220916110050778](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916110050778.png)



成功后可以从cmd中输入下面指令来查看当前设备

```
adb devices -l
```

![image-20220916110044793](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916110044793.png)





# 启动项目

确保打开模拟机或者连接usb真机

```npm
yarn android
```

注意要在工程项目里

```
cd [创建的工程文件夹]
```



![image-20220918133834009](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220918133834009.png)



# 问题

## 问题描述

![image-20220916110255356](C:\Users\FatDove\AppData\Roaming\Typora\typora-user-images\image-20220916110255356.png)



 Failed to install the app. Make sure you have the Android development environment set up: https://reactnative.dev/docs/environment-setup.
Error: Command failed: gradlew.bat app:installDebug -PreactNativeDevServerPort=8081

## 解决

没有使用jdk11的版本，默认使用的是jdk1.8

最后发现是之前设置环境变量的时候有一个JAVA_HOME设置路径为1.8所以一直报错










