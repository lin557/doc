# 防火墙配置

CentOS中防火墙程序主要是firewall和iptables，CentOS7 默认使用firewalld防火墙，而iptables服务需要自己用yum install iptabes-services来安装。这里只写关于firewalld的配置。CentOS7中的iptable指令是可用的，但并不是指iptable防火墙。

## firewalld

1. 查看防火墙状态
```
service firewalld status
或
firewalld-cmd --state
```
2. 启用防火墙
```
service firewalld start
或
systemctl start firewalld.service
```
3. 关闭防火墙
```
service firewalld stop
或
systemctl stop firewalld.service
```
4. 重启防火墙
```
service firewalld restart
或
firewall-cmd --reload
```
5. 禁止开机启动
```
systemctl disable firewalld.service
```
6. 开启开机启动
```
systemctl enable firewalld.service
```
7. 查看防火墙开放的端口列表, 默认禁用所有端口
```
firewall-cmd --list-ports
```
8. 将指定端口加入到防火墙中
```
firewall-cmd --zone=public --add-port=80/tcp --permanent

--zone=public(作用域)
--add-port=80/tcp(端口和访问类型)
--permanent(永久生效)
```
注意：添加后需要执行reload才能生效
```
firewall-cmd --reload
```
9. 将端口从防火墙中移除
```
firewall-cmd --zone=public --remove-port=80/tcp --permanent

--zone=public(作用域)
--remove-port (端口和访问类型)
--permanent(永久生效)
```
注意：移除后需要执行reload才能生效
```
firewall-cmd --reload
```