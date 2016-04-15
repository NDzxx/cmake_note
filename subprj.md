# 各个子工程目录
- 子工程目录文件夹包括
 - bqdata
 - bqnet
 - bqrpc
 - bqutil
 - cpp_redis
 - commonlibs
 - TestAsyncRpc
 - BqServer

细分为三类
- 编译为内部静态库 （包括bqdata bqnet bqrpc cpp_redis）
- 导入外部库 ( bqutil commonlibs)
- 生成可执行文件(TestAsyncRpc BqServer)

