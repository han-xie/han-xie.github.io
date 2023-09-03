---
layout: post
title: Linux命令行下的常用指令笔记
tags: [linux]
---

# 1. bash变量操作

1. `export PATH=${PATH#/some/path/*:}` 一个#号表示从最前面开始删除符合规则的最短的一个
2. `export PATH=${PATH##/*:}` 两个#号表示从最前面开始删除符合规则的最长的那个
3. `export PATH=${PATH%/some/path/*:}` 一个%号表示从最后面开始删除符合规则的最短的一个
4. `export PATH=${PATH%%/*:}` 两个%号表示从最后面开始删除符合规则的最长的那个
5. `${myvar/old/new}`替换第一个old为new
6. `${myvar//old/new}`替换所有old为new

**测试变量是否有内容并替换**

1. `newvar=${oldvar-root}`如果oldvar不存在则返回root，否则返回oldvar
2. `newvar=${oldvar:-root}`如果oldvar不存在或者为空字串则返回root，否则返回oldvar

以上内容可参考[鸟哥的linux私房菜](https://linux.vbird.org/)之[第十章、认识与学习Bash](https://linux.vbird.org/linux_basic/centos7/0320bash.php)。

# 2. 系统查看相关操作

1. 显示发行版本的所有信息：`lsb_release -a`；如果只需要显示发行版本号，可以使用`lsb_release -sr`；如果只需要显示发行代号，可以使用`lsb_release -sc`；以此类推...
2. 服务运行状态：`systemctl status some_service`，显示some_service的运行状态
3. `echo $?` 可以查看上一个程序结束后的返回值，详见[reference](https://superuser.com/questions/245048/how-to-check-for-program-exit-code-in-linux)
4. `nvidia-smi`查看显卡驱动有没有正常安装，`nvcc -V`查看cuda的信息

# 3. 编程相关指令

1. `c++filt`可以用于解析c++编译之后的函数名原型，详见[reference](https://blog.csdn.net/u013525455/article/details/78180614)。<br>
例如：`c++filt _Z3fooi`会返回foo(int)。
2. valgrind可以用于调试内存问题
3. 在x86机器上查看arm版本的.so依赖：`readelf -d libdd_asr.so | grep NEEDED`

# 4. 编码相关操作

1. `dos2unix`指令与`unix2dos`指令可以用来在DOS格式和Unix格式之间转换编码
2. `docker exec -e LANG=C.UTF-8 -it [container] bash` 使容器支持中文UTF-8

# 5. 文件和文件夹相关操作

1. `split`可以用来切割文件，然后用`cat`合并到一起
2. `basename`用于去除**路径**和**文件后缀**部分的文件名或者目录名
3. `dirname`用于从文件路径中获取文件目录
4. `file`用于[辨识文件类型](https://www.runoob.com/linux/linux-comm-file.html)，而且可以借助范围指定符列出以指定范围字母开头的文件的文件类型，详见[reference](https://learnku.com/server/wikis/36526)

