# 编译为内部静态库
##1.bqdata   
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

#set_property(TARGET ${lib_name} PROPERTY FOLDER ${ARGV2}) 见下面分析
```

###source_group(str FILES var)
在vs下生成工程组，
如下图  
![解决方案图示](group.jpg)  

###set_property  
在给定的作用域内设置一个命名的属性。

  set_property(<GLOBAL                            |
                DIRECTORY [dir]                   |
                TARGET    [target1 [target2 ...]] |
                SOURCE    [src1 [src2 ...]]       |
                TEST      [test1 [test2 ...]]     |
                CACHE     [entry1 [entry2 ...]]>
               [APPEND]
               PROPERTY <name> [value1 [value2 ...]])

　　为作用域里的0个或多个对象设置一种属性。第一个参数决定了属性可以影响到的作用域。他必须是下述值之一：GLOBAL，全局作用域，唯一，并且不接受名字。
  - DIRECTORY，路径作用域，默认为当前路径，但是也可以用全路径或相对路径指定其他值。
  - TARGET，目标作用域，可以命名0个或多个已有的目标。
  - SOURCE，源作用域，可以命名0个或多个源文件。注意，源文件属性只对加到相同路径（CMakeLists.txt）中的目标是可见的。
  - TEST 测试作用域可以命名0个或多个已有的测试。CACHE作用域必须指定0个或多个cache中已有的条目。  
  - PROPERTY选项是必须的，并且要紧跟在待设置的属性的后面。剩余的参数用来组成属性值，该属性值是一个以分号分隔的list。如果指定了APPEND选项，该list将会附加在已有的属性值之后。
   - PROPERTY FOLDER 设置文件夹路径  
  **相应的可以使用get_property:获取一个属性值  **
  
##2.bqnet

