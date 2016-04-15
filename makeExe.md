# 编译可执行文件
##TestAsyncRpc
```
include_directories(
. 
.. 
../commonlibs/dep/include 
../commonlibs/dep/include/mysql 
../bqutil
)

file(GLOB HEADER_LIST ./*.h)
file(GLOB SRC_LIST ./*.cpp)

set(allFiles ${HEADER_LIST} ${SRC_LIST})
source_group("include" FILES ${HEADER_LIST})
source_group("source" FILES ${SRC_LIST})
#编译成可执行文件
add_executable(TestAsyncRpc ${allFiles})
#链接之前编译好的静态库，内部编译的静态库只需要库名即可链接
target_link_libraries(TestAsyncRpc bqnet)
target_link_libraries(TestAsyncRpc bqutil)
target_link_libraries(TestAsyncRpc bqrpc)
target_link_libraries(TestAsyncRpc cpp_redis)
```
##BqServer
```
include_directories(
. 
../ 
../commonlibs/dep/include 
../commonlibs/dep/include/mysql 
../bqutil 
)

file(GLOB HEADER_LIST ./*.h)
file(GLOB SRC_LIST ./*.cpp)
file(GLOB_RECURSE DataCenterFiles DataCenter/*.h DataCenter/*.cpp)
file(GLOB_RECURSE DataStructFiles DataStruct/*.h DataStruct/*.cpp)
file(GLOB_RECURSE GateServiceFiles GateService/*.h GateService/*.cpp)
file(GLOB_RECURSE HttpServiceFiles HttpService/*.h HttpService/*.cpp)
file(GLOB_RECURSE LoginServiceFiles LoginService/*.h LoginService/*.cpp)
file(GLOB_RECURSE NetCryptorInclude NetCryptor/*.h)
#只显示，不编译，因为下面有exe程序
file(GLOB_RECURSE PbDefFiles PbDef/*.proto PbDef/makefile)
file(GLOB_RECURSE PbSrcFiles PbSrc/*.*)

set(
 allFiles
 ${HEADER_LIST}
 ${SRC_LIST}
 ${DataCenterFiles}
 ${DataStructFiles}
 ${GateServiceFiles}
 ${HttpServiceFiles}
 ${LoginServiceFiles}
 ${PbSrcFiles}
 ${NetCryptorInclude}
 )
source_group("include" FILES ${HEADER_LIST})
source_group("source" FILES ${SRC_LIST})
source_group("DataCenter" FILES ${DataCenterFiles})
source_group("DataStruct" FILES ${DataStructFiles})
source_group("GateService" FILES ${GateServiceFiles})
source_group("HttpService" FILES ${HttpServiceFiles})
source_group("LoginService" FILES ${LoginServiceFiles})
source_group("PbDef" FILES ${PbDefFiles})
source_group("PbSrc" FILES ${PbSrcFiles})
source_group("NetCryptorInclude" FILES ${NetCryptorInclude})
add_executable(BqServer ${allFiles})

if (CMAKE_SYSTEM_NAME MATCHES "Windows")
 target_link_libraries(BqServer ws2_32)
elseif (CMAKE_SYSTEM_NAME MATCHES "Linux")
 message(STATUS "linux socke lib?")  
 #准备加入linux的.a库
else()
 message(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")  
endif (CMAKE_SYSTEM_NAME MATCHES "Windows") 
#外部库依旧需要使用搜索，话说可以使用link_directories试试，省不少代码
#搜索lib_protobuf
FIND_PACKAGE(lib_protobuf REQUIRED)
MARK_AS_ADVANCED(
 LIB_PROTOBUF_LIBRARIES
)

IF(LIB_PROTOBUF_LIBRARIES)
MESSAGE(STATUS "Found lib_Protobuf libraries")
target_link_libraries(BqServer ${LIB_PROTOBUF_LIBRARIES})
ENDIF (LIB_PROTOBUF_LIBRARIES)

#搜索lib_dllALL
FIND_PACKAGE(lib_dllAll REQUIRED)
MARK_AS_ADVANCED(
 LIB_EAY32_LIBRARIES
)

IF(LIB_EAY32_LIBRARIES)
MESSAGE(STATUS "Found lib_dllAll libraries")
target_link_libraries(BqServer ${LIB_EAY32_LIBRARIES})
ENDIF (LIB_EAY32_LIBRARIES)

#搜索NetCryptor
FIND_PACKAGE(lib_NetCryptor REQUIRED)
MARK_AS_ADVANCED(
 LIB_NETCRYPTOR_LIBRARIES
)

IF(LIB_NETCRYPTOR_LIBRARIES)
MESSAGE(STATUS "Found lib_NetCryptor libraries")
target_link_libraries(BqServer ${LIB_NETCRYPTOR_LIBRARIES})
ENDIF (LIB_NETCRYPTOR_LIBRARIES)

add_dependencies(BqServer bqdata)
add_dependencies(BqServer bqnet)
add_dependencies(BqServer bqutil)
add_dependencies(BqServer bqrpc)
add_dependencies(BqServer cpp_redis)

target_link_libraries(BqServer bqdata)
target_link_libraries(BqServer bqnet)
target_link_libraries(BqServer bqutil)
target_link_libraries(BqServer bqrpc)
target_link_libraries(BqServer cpp_redis)
add_dependencies(BqServer bqdata)
```