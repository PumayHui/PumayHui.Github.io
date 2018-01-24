---
layout:     post
title:      "vs2012在Windows下编译Thrift 0.10.0并实现C++与Python的双向通信"
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
- libevent-2.0.21-stable（libthriftnb用到）
- Python3.5.4

# 二. 编译步骤

1. 编译boost，见[windows 下boost库的简单编译](http://blog.csdn.net/wap1981314/article/details/12138617)；
2. 编译OpenSSL，见[在 Windows下用 Visual Studio 编译 OpenSSL 1.1.0 ](https://www.cnblogs.com/chinalantian/p/5819105.html);
3. 去{thrift 安装目录}\lib\cpp 目录，点击thrift.sln，打开 VS 项目，里边有两个项目libthrift（只用到这个） 和 libthriftnb；
4. 点击升级；
5. libthrift工程配置：

    libthrift>属性->C/C++->代码生成->多线程调试
    
    libthrift>属性->C/C++->常规->附加包含目录->\boost_1_53_0
    
    libthrift>属性->C/C++->常规->附加包含目录->\openssl-1.1.0\include
    
    libthrift>属性->库管理器->常规->附加库目录->\boost_1_53_0\lib\vc11_x86\lib
    
    libthrift>属性->库管理器->常规->附加库目录->\openssl-1.1.0
    
    libthrift>属性->库管理器->所有选项->libssl.lib
                                      libcrypto.lib

    编译完成后，在\thrift-0.10.0\lib\cpp\Debug下生成libthrift.lib文件和libthriftnb.lib文件。

# 三. 配置

1. 编写.thrift文件：
    ```
    service HelloWorldBidirectionService{
        oneway void SayHello(1:string msg);
    }
    ```
2. 用thrift.exe生成gen-cpp目录（C++，server端）、gen-py文件（Python，client端）：

    `thrift --gen cpp hello.thrift`
    
    `thrift --gen py hello.thrift`
    
3. 新建server项目，将gen-cpp内文件添加到项目；
4. server项目配置：

    server>属性->C/C++->常规->附加包含目录->\boost_1_53_0
    
    server>属性->C/C++->常规->附加包含目录->\thrift-0.10.0\lib\cpp\src
    
    server>属性->C/C++->常规->附加包含目录->\thrift-0.10.0\lib\cpp\src\thrift\windows
    
    server>属性->C/C++->代码生成->多线程调试
    
    server>属性->链接器->常规->附加库目录->\boost_1_53_0\lib\vc11_x86\lib
    
    server>属性->链接器->常规->附加库目录->\thrift-0.10.0\lib\cpp\Debug
    
5. client端：

    1）打开cmd窗口，定位到gen-py所在目录；
    
    2）新建client.py文件，添加代码：
    ```
    #!/usr/bin/env python
    #coding:utf-8
    import sys
    sys.path.append('./')

    from thrift import Thrift
    from thrift.transport import TSocket
    from thrift.transport import TTransport
    from thrift.protocol import TBinaryProtocol

    from HelloWorldBidirection import HelloWorldBidirectionService

    def main():
        try:
            socket = TSocket.TSocket('localhost', 9092)
            transport = TTransport.TBufferedTransport(socket)
            protocol = TBinaryProtocol.TBinaryProtocol(transport)
            client = HelloWorldBidirectionService.Client(protocol)
            transport.open()

            # 接收server端发送的消息
            handler = HelloWorldBidirectionService.Iface()
            inProtocol = TBinaryProtocol.TBinaryProtocol(socket)
            outProtocol = TBinaryProtocol.TBinaryProtocol(socket)
            processor = HelloWorldBidirectionService.Processor(handler)
            processor.process(inProtocol, outProtocol)

            while (1):
                str = input("input msg: ")
                msg_ret = client.SayHello(str)
                print(str)

            transport.close()
        except Thrift.TException as ex:
            print("Connection closed")
            print("%s" % (ex.message))

    if __name__ == '__main__':
        main()
    ```
6. 通信：

    1）打开server工程下的debug目录，点击server.exe打开server端窗口；
    
    2）在cmd输入：
        `python client.py`
       开始运行client端。
