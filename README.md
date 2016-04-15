# cmake note

网上写的cmake知识感觉很零碎，这几天刚好在做cmake，结合自己做的整理一份笔记出来，以后有用到也方便查看
cmake并不注重大小写，我自己写的时候因为是网络查找得来，大小写混用照样过得去
安装win什么的不说了，直接下exe即可
ubuntu:  

```shell
 #cmake安装
 sudo apt-get install cmake  
 
 #cmake gui安装
 sudo apt-get install cmake-qt-gui  
```

```
 #如果需要卸载cmake
 apt-get autoremove cmake
```
注:vs2015自带的调试->诊断工具查找内存泄漏很好用

##ubuntu下mysql安装和启动
- 安装

```
sudo apt-get install mysql-server
 
apt-get isntall mysql-client
 
sudo apt-get install libmysqlclient-dev
```
- 启动
```
sudo /etc/init.d/mysql start
```
- 停止
```
sudo /etc/init.d/mysql stop
```
- 重启
```
sudo /etc/init.d/mysql restart
```
- 查看日志  

```
cat /var/log/mysql.err
 
cat /var/log/mysql/error.log
```
- 远程连接
###授权
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'  
IDENTIFIED BY 'admin123'  WITH GRANT OPTION;
flush privileges;
```
###设置
```
sudo gedit  /etc/mysql/my.cnf
```
打开后注释掉 bind-address		= 127.0.0.1这一行
然后重启即可
- 连接测试
	```
    mysql -h 192.168.228.130 -u root -p root
    ```

