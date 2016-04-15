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
set(allFiles ${HEADER_LIST} ${SRC_LIST})
add_library(bqdata STATIC ${allFiles})
```
2.bqnet

