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
add_executable(TestAsyncRpc ${allFiles})

target_link_libraries(TestAsyncRpc bqnet)
target_link_libraries(TestAsyncRpc bqutil)
target_link_libraries(TestAsyncRpc bqrpc)
target_link_libraries(TestAsyncRpc cpp_redis)
```
