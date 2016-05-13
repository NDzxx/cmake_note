# mfc

cmakes生成mfc程序的几个注意点  
- SET(CMAKE_MFC_FLAG 2) 
 - 0 标准win程序
 - 1 静态库  
 - 2 动态库  
在生成exe的地方这样写  
if (CMAKE_SYSTEM_NAME MATCHES "Windows")     
file(GLOB RES_LIST ./BoxAccount.rc) ---这里要加入rc  
source_group("Res" FILES ${RES_LIST})  
add_executable(BoxAccount WIN32 ${allFiles} ${RES_LIST})  
endif (CMAKE_SYSTEM_NAME MATCHES "Windows") 