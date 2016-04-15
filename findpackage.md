# 查找引用外部库文件
##4.bqutil
```
include_directories(
. 
./http 
.. 
../commonlibs/dep/include
../commonlibs/dep/include/mysql
../bqutil
)

ADD_DEFINITIONS(-DSTATIC_LIBMONGOCLIENT)
ADD_DEFINITIONS(-DBUILDING_LIBCURL)
ADD_DEFINITIONS(-DDEBUGBUILD)
ADD_DEFINITIONS(-DCURL_STATICLIB)
ADD_DEFINITIONS(-DUSE_OPENSSL)

file(GLOB HEADER_LIST ./*.h)
file(GLOB SRC_LIST ./*.cpp)
file(GLOB_RECURSE httpFiles ./http/*.cpp ./http/*.h)

set(allFiles ${HEADER_LIST} ${SRC_LIST} ${httpFiles})
source_group("include" FILES ${HEADER_LIST})
source_group("source" FILES ${SRC_LIST})
source_group("http" FILES ${httpFiles})
add_library(bqutil STATIC ${allFiles})

#查找自定义库
FIND_PACKAGE(lib_curl_ssl_md REQUIRED)
MARK_AS_ADVANCED(
 LIB_CURLSSL_LIBRARIES
)

IF(LIB_CURLSSL_LIBRARIES)
MESSAGE(STATUS "Found lib_curl libraries")
TARGET_LINK_LIBRARIES(bqutil ${LIB_CURLSSL_LIBRARIES})
ENDIF (LIB_CURLSSL_LIBRARIES)
```
##link_directories
在多个模块的情况下，可能一个模块的链接依赖於其它模块，例如一个可执行二进制需要链接某些模块，
此时link_directories将有发挥作用。
如在CMakeLists.txt增加：
link_directories(${MyProject_BINARY_DIR}/src/libxxx
${MyProject_BINARY_DIR}/src/libyyy)
将指示CMake在LDFLAGS附加-Lsrc/libxxx -Lsrc/libyyy。  
PS:baidu说此种方法有bug(?未验证)，最好采用find_package,所以我在工程中使用了find_package
##FIND_PACKAGE


