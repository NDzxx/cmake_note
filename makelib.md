# 编译为内部静态库
1.bqdata   
CMakelists.txt如下  
```cmake
#包含的头文件
include_directories(
. 
../
../commonlibs/dep/include 
../commonlibs/dep/include/mysql
../bqutil
)
#额外的预处理器
ADD_DEFINITIONS(-DSTATIC_LIBMONGOCLIENT)

#归类查找文件并赋值给变量
#FILE(GLOB var source) 在当前目录下查找
#FILE(GLOB_RECURSE var [RELATIVE path] 包含子文件夹的查找方式
#aux_source_directory(path var) 查找对应路径下所有的文件并赋值给var
FILE(GLOB SRC_LIST ./*.cpp)
FILE(GLOB HEADER_LIST ./*.h)


source_group("Include" FILES ${HEADER_LIST})
source_group("Source" FILES ${SRC_LIST})
#把分散文件合成一个变量
set(allFiles ${HEADER_LIST} ${SRC_LIST})
#编译静态库
#如果编译动态库，如dll或者so,只需要修改STATIC为SHARED
add_library(bqdata STATIC ${allFiles})
```

source_group(str FILES var)在vs下生成工程组，
如下图
![解决方案图示](group.jpg)  

2.bqnet

