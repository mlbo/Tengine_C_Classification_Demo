# Tengine Classification C X86版本

## 环境
**System**: Ubuntu20.04

**CMAKE**: 3.16.3

## 准备

1. 把[第0篇-Tengine X86版本编译](https://zhuanlan.zhihu.com/p/181579203)中编译好的so和头文件复制到项目目录中的tengine目录
2. 把[Tengine/examples/common](https://github.com/OAID/Tengine/tree/tengine-lite/examples/common)中部分文件放入common目录
3. 把[模型仓库](https://github.com/OAID/Tengine/tree/tengine-lite/examples#%E6%A8%A1%E5%9E%8B%E4%BB%93%E5%BA%93)的mobilenet.tmfile模型放到models目录，cat.jpg图片放到images目录
4. 把[tm_classification.c](https://github.com/OAID/Tengine/blob/tengine-lite/examples/tm_classification.c)文件放到src目录

此时目录结构应该是这样

```
├── common
│   ├── common.h
│   ├── stb_image.h
│   ├── stb_image_write.h
│   ├── tengine_operations.c
│   └── tengine_operations.h
├── imgs
│   └── cat.jpg
├── models
│   └── mobilenet.tmfile
├── run.sh
├── src
│   └── tm_classification.c
└── tengine
    ├── include
    │   └── tengine
    │       ├── c_api.h
    │       └── defines.h
    └── lib
        └── libtengine-lite.so
```
## 编写 CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.10)
project(tm_classification)

set(CMAKE_CXX_STANDARD 11)

include_directories(tengine/include/ common/)

add_library(tengine SHARED IMPORTED)
set_target_properties(tengine PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/tengine/lib/libtengine-lite.so)

add_executable(tm_classification src/tm_classification.c  "common/tengine_operations.c" )

target_link_libraries(tm_classification -lm tengine)

```

## 编译

```bash
mkdir build
cd build
cmake ..
make -j4
```

## 测试

```
./build/tm_classification -m ./models/mobilenet.tmfile -i ./images/cat.jpg 
```
输出
```
Image height not specified, use default 224
Image width not specified, use default  224
Scale value not specified, use default  0.0, 0.0, 0.0
Mean value not specified, use default   104.0, 116.7, 122.7
tengine-lite library version: 1.4-dev

model file : ./models/mobilenet.tmfile
image file : ./imgs/cat.jpg
img_h, img_w, scale[3], mean[3] : 224 224 , 0.017 0.017 0.017, 104.0 116.7 122.7
Repeat 1 times, thread 1, avg time 42.71 ms, max_time 42.71 ms, min_time 42.71 ms
--------------------------------------
8.574144, 282
7.880117, 277
7.812573, 278
7.286458, 263
6.357486, 281
--------------------------------------
```

## 参考

1. [Tengine 人脸检测X86版本](https://github.com/jiangzhongbo/Tengine_Tutorial/tree/master/2_FaceDetector)
2. [https://github.com/OAID/Tengine](https://github.com/OAID/Tengine)