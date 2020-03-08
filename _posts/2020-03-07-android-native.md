---
layout: post
title: Notes on Android native programming
tags: [note, programming]
---

## 1. Android adb shell执行程序

在一些开发板上，交叉编译的可执行程序可以在Android adb连接到的shell里直接运行。但是需要把可执行文件放在/dev文件夹下面，RK3399 Pro开发板上亲测可用 [[Reference](https://blog.csdn.net/wlwh90/article/details/45561679)]。

```bash
$ adb push a.out /dev
$ adb shell
$ cd /dev
$ ./a.out
```

## 2. 使用cmake交叉编译找不到std的库函数

原因是cmake没有找到相应的std库，需要在执行cmake的时候加上`-DANDROID_STL=c++_shared`或者`-DANDROID_STL=c++_static`选项 [[Reference](https://stackoverflow.com/questions/41603049/error-no-member-named-to-string-in-namespace-std-did-you-mean-tostring)]。

举例：cmake编译armv7的Android库，注意加了`-DANDROID_STL=c++_static`

```bash
cmake \
  -DANDROID_ABI=armeabi-v7a \
  -DANDROID_PLATFORM=android-21 \
  -DANDROID_NDK=$Some_Path/android-ndk-r16b \
  -DCMAKE_TOOLCHAIN_FILE=$Some_Path/android-ndk-r16b/build/cmake/android.toolchain.cmake \
  -DBUILD_ANDROID=true \
  -DBUILD_TESTING=true \
  -DANDROID_TOOLCHAIN=clang \
  -DANDROID_STL=c++_static ..
```

关于ndk使用cmake编译的更多选项可以参考 [link](https://developer.android.com/ndk/guides/cmake.html#variables)。

## 3. 出现undefined reference to 'rand'等函数

rand函数应该是数学库里的，ndk应该会自动链接`libm.so`，出现这种情况很有可能是因为Andoid Platform跟之前其他依赖库选择的不一致 [[Reference](https://www.cnblogs.com/yuandaozhe/p/5892118.html)]。