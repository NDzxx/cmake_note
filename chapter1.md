# 主工程根目录

```cmake
cmake_minimum_required(VERSION 2.8)
#工程名，就是vs的sln文件
project(AsyncRpc)

# 可选项编译的是x64还是x86?
option (USE_X64 "Use Platform x64" ON)
if(USE_X64)
 set(PLATFORM x64)
else()
  set(PLATFORM x86)
endif(USE_X64)

#如果没选择编译选项，默认debug
if( NOT CMAKE_BUILD_TYPE)
  set( CMAKE_BUILD_TYPE Debug CACHE STRING
       "Choose the type of build, options are:\
       None Debug Release RelWithDebInfo MinSizeRel." 
       FORCE )
endif()

#根据不同的操作系统选择编译选项
message(STATUS "operation system is ${CMAKE_SYSTEM}")  
if (CMAKE_SYSTEM_NAME MATCHES "Linux")  
    message(STATUS "current platform: Linux ")
    set(CMAKE_VERBOSE_MAKEFILE on)
    set(CMAKE_CXX_COMPILER "g++")
    #使用c++11
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}-std=c++11") 
    #根据debug和release使用不同优化
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -O0 -Wall -g -ggdb")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O3 -Wall -DNDEBUG")
    #64位还是32位程序选择
	if(USE_X64)
	 set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m64")
    else()
      set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
	endif (USE_X64)
elseif (CMAKE_SYSTEM_NAME MATCHES "Windows")  
    message(STATUS "current platform: Windows")
    #ADD_DEFINITIONS(-D_AFXDLL)
    #mfc使用选项 0是不使用，1是静态使用mfc库 2是dll使用mfc库
    SET(CMAKE_MFC_FLAG 0)
else()  
    message(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")  
endif (CMAKE_SYSTEM_NAME MATCHES "Linux") 
#开启相对路径选项
set(CMAKE_USE_RELATIVE_PATHS ON)
#库输出路径设置
set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/lib/)

#额外预处理器
ADD_DEFINITIONS(-DCRT_SECURE_NO_WARNINGS)
ADD_DEFINITIONS(-DASIO_STANDALONE)
ADD_DEFINITIONS(-DASIO_HAS_STD_CHRONO)

#函数例子 本工程基本没有使用
function(assign_source_group)
    foreach(_source IN ITEMS ${ARGN})
        if (IS_ABSOLUTE "${_source}")
            file(RELATIVE_PATH _source_rel "${CMAKE_CURRENT_SOURCE_DIR}" "${_source}")
        else()
            set(source_rel "${_source}")
        endif()
        get_filename_component(_source_path "${_source_rel}" PATH)     
        string(REPLACE "/" "\\" _source_path_msvc "${_source_path}")
        source_group("${_source_path_msvc}" FILES "${_source}")
    endforeach()
endfunction(assign_source_group)

function(create_lib source_dir lib_name)
	#message(${source_dir})
	set(exp ${source_dir}/*.h* ${source_dir}/*.c*)
	file(GLOB_RECURSE allFiles ${exp})
	#message(aa ${allFiles})	
	assign_source_group(${allFiles})
	Add_LIBRARY(${lib_name} STATIC ${allFiles})
	if(ARGV2)
		set_property(TARGET ${lib_name} PROPERTY FOLDER ${ARGV2})
	endif()
endfunction(create_lib)
#设置查找库的cmake路径
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
#增加子目录 具体功能在子目录CMakelists.txt编写
add_subdirectory(bqdata)
add_subdirectory(bqnet)
add_subdirectory(bqrpc)
add_subdirectory(bqutil)
add_subdirectory(cpp_redis)
add_subdirectory(TestAsyncRpc)
add_subdirectory(BqServer)

```
