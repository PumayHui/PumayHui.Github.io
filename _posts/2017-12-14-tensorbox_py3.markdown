---
layout:     post
title:      "TensorBox python3使用注意事项"
date:       2017-12-14 10:16:00
author:     "Pumay"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
tags:
    - TensorFlow
    - python
    - C++
    - Cython
    
---

# 平台：

- Windows 64bit

- VS2015

- anaconda python3.5.0

- TensorFlow 1.1.0

# 注意事项

## 下载源码后编译（Windows）：

在anaconda prompt输入：

`cd TensorBox\utils `

`cmake -DPYTHON_INCLUDE_DIR=$(python -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") -DPYTHON_LIBRARY=$(python -c "import distutils.sysconfig as sysconfig; print(sysconfig.get_config_var('LIBDIR'))") CMakeLists.txt `

新建setup.py文件，内容如下：

```
from distutils.core import setup, Extension
from Cython.Build import cythonize

# First create an Extension object with the appropriate name and sources
ext = Extension(name="stitch_wrapper",
                sources=["stitch_rects.cpp", "stitch_wrapper.cpp", "./hungarian/hungarian.cpp"])

# Use cythonize on the extension object.
setup(ext_modules=cythonize(ext))

```
继续在anaconda prompt输入：

`python setup.py build `

`python setup.py install`

即可生成stitch_wrapper.pyd文件。

