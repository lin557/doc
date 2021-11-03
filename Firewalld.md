# 防火墙Firewalld配置

CentOS中防火墙程序主要是firewall和iptables，CentOS7 默认使用firewalld防火墙，而iptables服务需要自己用yum install iptabes-services来安装。这里只写关于firewalld的配置。CentOS7中的iptable指令是可用的，但并不是指iptable防火墙。



## 开启与关闭



### 1. 查看状态

```
systemctl status firewalld
或
service firewalld status
或
firewalld-cmd --state
```


### 2. 启动防火墙

```
service firewalld start
或
systemctl start firewalld.service
```


### 3. 关闭防火墙

```
service firewalld stop
或
systemctl stop firewalld.service
```


### 4. 重启防火墙

```
service firewalld restart
或
firewall-cmd --reload
```
### 5. 禁止开机启动
```
systemctl disable firewalld.service
```
### 6. 开启开机启动
```
systemctl enable firewalld.service
```



### 7. 查看防火墙规则

```
# 查看防火墙开放的端口列表, 默认禁用所有端口

# 查看开放端口
firewall-cmd --list-ports

# 查看转发端口
firewall-cmd --list-forward-ports

# 所有规则
firewall-cmd --list-all

# 规则文件
cat /etc/firewalld/zones/public.xml
```



## 开放端口



### 1. 开放指定端口

```
# --zone=public(作用域)
# --add-port=80/tcp(端口和访问类型)
# --permanent(永久生效)

firewall-cmd --zone=public --permanent --add-port=80/tcp 

# 多个端口
firewall-cmd --zone=public --permanent --add-port=100-500/tcp

# 注意：添加后需要执行reload才能生效
firewall-cmd --reload

# 可以用以下命令检查是否成功
firewall-cmd --list-ports
```



### 2. 将端口从防火墙中移除

```
# --zone=public(作用域)
# --remove-port (端口和访问类型)
# --permanent(永久生效)
firewall-cmd --zone=public --remove-port=80/tcp --permanent

# 注意：添加后需要执行reload才能生效
firewall-cmd --reload
```



### 3. 添加端口转发

```
# 将访问端口9020的数据 转发到端口 17020
firewall-cmd --zone=public --add-forward-port=port=9020:proto=tcp:toport=17020 --permanent

# 转发到其他服务器
firewall-cmd --zone=public --add-forward-port=port=9180:proto=tcp:toaddr=127.0.0.1:toport=17180 --permanent
```



### 4. 转发某段IP数据包

```
# 转发某个IP的包到某台服务器的某个端口上
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="192.168.0.5" forward-port port=9900 protocol=tcp to-addr="119.28.187.174" to-port=80'

# 转发某段IP的包到某台服务器的某个端口上
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="192.168.0.0/24" forward-port port=9900 protocol=tcp to-addr="119.28.187.174" to-port=80'

firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="10.144.110.6" forward-port port=7000 protocol=tcp to-addr="119.28.187.174" to-port=7000'

firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="10.144.110.6" forward-port port=7001 protocol=tcp to-addr="119.28.187.174" to-port=7001'

firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="10.144.110.6" forward-port port=7002 protocol=tcp to-addr="119.28.187.174" to-port=7002'

firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="10.144.110.6" forward-port port=7003 protocol=tcp to-addr="119.28.187.174" to-port=7003'

firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="10.144.110.6" forward-port port=7004 protocol=tcp to-addr="119.28.187.174" to-port=7004'

firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="10.144.110.6" forward-port port=7005 protocol=tcp to-addr="119.28.187.174" to-port=7005'
```



### 4. 禁止IP访问

```
# 禁止IP(123.44.55.66)访问机器
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="123.44.55.66" drop'
# 禁止一个IP段，比如禁止116.255.*.*
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="116.255.0.0/16" drop'
# 禁止一个IP段，比如禁止116.255.196.*
firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address="116.255.196.0/24" drop'

# 禁止机器IP(123.44.55.66)从防火墙中删除
firewall-cmd --permanent --remove-rich-rule='rule family=ipv4 source address="123.44.55.66" drop'
```



### 5. 开放指定协议

```
```



## 异常处理



### 1. allowzonedrifting

```
# 启动后提示
Warning:allowzonedrifting is enabled.This is considered an insecure configuration option.It will be removed  in a future release ... ling it now
```

~~~
# 修改配置文件
vim /etc/firewalld/firewalld.conf

# 搜索：AllowZoneDrifiting 把对应的值 yes 改为 no
AllowZoneDrifiting=no

# :wq 退出并保存 重启防火墙
~~~

