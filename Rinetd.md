# Rinetd端口映射软件



## 安装

1. 下载
```
官网下载 https://boutell.com/rinetd/ 
Linux版本 https://boutell.com/rinetd/http/rinetd.tar.gz
```
如果没有安装wget, 可以先安装, 安装过程按 y 同意安装
```
yum install wget
```
```
cd /usr/local/src
wget http://boutell.com/rinetd/http/rinetd.tar.gz
```
注: 我用wget下载下来的文件一直是坏的, 后来用迅雷下载，然后上传到 /usr/local/src
2. 解压文件
```
cd /usr/local/src
tar -zxvf rinetd.tar.gz
```
3. 编译
进入源码目录
```
cd /usr/local/src/rinetd
```
执行编译
```
make
```
创建目录 /usr/man/man8 否则执行make install时会报错
```
mkdir /usr/man
mkdir /usr/man/man8
```
执行安装
```
make install
```
安装成功，安装目录在 /usr/sbin
```
[root@vdo rinetd]# make install
install -m 700 rinetd /usr/sbin
install -m 644 rinetd.8 /usr/man/man8
```



## 配置

1. 新建配置文件 /etc/rinetd.conf
```
vim /etc/rinetd.conf
```
2. 输入以下内容, 按 insert 进入输入模式
```
格式: <绑定地址> <端口> <转发到指定的地址> <端口>
例如: 10.144.110.6 7000 192.168.0.9 8000
表示: 将访问 10.144.110.6:7000 的数据，自动映射到 192.168.0.9:8000
```
```
10.144.110.6 7000 192.168.0.9 8000
// 如果有多笔
10.144.110.6 7001 192.168.0.9 8001
10.144.110.6 7002 192.168.0.9 8002
```
按 ESC 返回命令模式, 按 :wq 保存退出

## 启动

1. 进入安装目录
```
cd /usr/sbin
```
2. 执行启动命令
```
rinetd -c /etc/rinetd.conf
```

## 关闭

1. 方法一
```
pkill rinetd
```
2. 方法二

找出pid
```
ps -ef|grep rinetd
```
直接kill
```
kill -9 pid
```