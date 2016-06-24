# release和debug
在cmake中要编译debug模式的话，在CMakeLists.txt中添加如下两行
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb ")
SET(CMAKE_CXX_FLAGS_RELEASE "${ENV{CXXFLAGS} -O3 -Wall")

然后，在编译的时候，使用如下命令：  
cmake -DCMAKE_BUILD_TYPE=Debug/Release  path

第三个参数path是指项目的顶层路径

