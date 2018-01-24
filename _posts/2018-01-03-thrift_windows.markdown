---
layout:     post
title:      "利用vs2012在Windows下编译Thrift 0.10.0"
date:       2018-01-23 17:53:18
author:     "Pumay"
header-img: "img/star.png"
catalog: true
tags:
    - Programming Language
    
---


# 一. 环境

- Windows 7 64bit
- visual studio 2012
- thrift 0.10.0
- boost_1_53_0
- openssl-1.1.0
- libevent-2.0.21-stable

# 二. 编译步骤

1. 编译boost，见[windows 下boost库的简单编译](http://blog.csdn.net/wap1981314/article/details/12138617)；
2. 编译OpenSSL，见[在 Windows下用 Visual Studio 编译 OpenSSL 1.1.0 ](https://www.cnblogs.com/chinalantian/p/5819105.html);
3. 去{thrift 安裝目錄}\lib\cpp 目录，点击thrift.sln，打开 VS 项目，里边有两个项目libthrift（只用到这个） 和 libthriftnb；
4. 点击升级；
5. libthrift工程配置：

libthrift>属性->C/C++->代码生成->多线程调试

libthrift>属性->C/C++->常规->附加包含目录->\boost_1_53_0；openssl-1.1.0\include

libthrift>属性->库管理器->常规->附加库目录->\boost_1_53_0\lib\vc11_x86\lib;D:\packages\openssl-1.1.0

libthrift>属性->库管理器->所有选项->libssl.lib;libcrypto.lib

编译完成后，在\thrift-0.10.0\lib\cpp\Debug下生成libthrift.lib文件和libthriftnb.lib文件。

# 三. 配置
