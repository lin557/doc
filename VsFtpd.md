# vsftpd的安装与配置



## 安装

```
yum -y install vsftpd
```



## 服务与启动

### 1. 启动

```
# centos7
systemctl start vsftpd.service
```

```
# centos6
service vsftpd start 
```

### 2. 停止

```
# centos7
systemctl stop vsftpd.service
```

### 3. 重启

```
# centos7
systemctl restart vsftpd.service
```

### 4. 开机自启

```
# centos7
systemctl enable vsftpd.service
```

```
# 查看开机自启列表
systemctl -l
# 查看vsftpd是否在开机启动列表中
systemctl -l | grep vsftpd
```



## 配置

```
# 默认配置文件位置
vim /etc/vsftpd/vsftpd.conf
```



### 1. 端口

```
# 默认21, 这里将默认端口改为 25956
listen_port=25956
```



### 2. 被动模式

```
# 被动模式
pasv_enable=YES

# 被动端口 必须大于1024
pasv_min_port=56000, pasv_max_port=56199
```



### 3. 匿名配置

```
# 允许匿名访问 默认YES
anonymous_enable=YES

# 匿名用户根目录
anon_root=/data/ftp

# 允许上传文件 另外linux文件夹要设置 747 权限
anon_upload_enable=YES

# 匿名用户删除 改名权限 默认NO
anon_other_write_enable=NO

# 禁止匿名用户下载权限 默认YES NO=允许下载
anon_world_readable_only=NO
```



### 4. 日志

```
# 启用上传下载日志 只记录文件信息
xferlog_enable=YES
xferlog_std_format=YES
# 指定日志文件位置
xferlog_file=/data/logs/vsftpd/xferlog

# 启用双份日志。在用xferlog文件记录服务器上传下载情况的同时，
# vsftpd_log_file所指定的文件，即/var/log/vsftpd.log也将用来 记录服务器的传输情况
dual_log_enable=YES
vsftpd_log_file=/data/logs/vsftpd/vsftpd.log
```



```
# xferlog 文件信息
Wed Jun  9 09:03:49 2021 11 ::ffff:110.49.215.249 3746585 /LogFile/77077-2021_6_9_9_3_42_message.log b _ i a ? ftp 0 * c
Wed Jun  9 09:04:23 2021 29 ::ffff:110.49.215.249 10485248 /LogFile/77077_BusRec_2021.6.9.9.3.52 b _ i a ? ftp 0 * c
Wed Jun  9 17:59:25 2021 1 ::ffff:58.61.50.232 8191 /update/L12-1-V05ysqy0607.sw b _ i a anonymous@example.com ftp 0 * c
Wed Jun  9 18:00:24 2021 1 ::ffff:58.61.50.232 8191 /update/update.sw b _ i a anonymous@example.com ftp 0 * c
Thu Jun 10 08:52:05 2021 1 ::ffff:110.49.215.249 0 /update/update.sw b _ o a ? ftp 0 * i
Thu Jun 10 08:52:06 2021 1 ::ffff:110.49.215.249 0 /update/update.sw b _ o a ? ftp 0 * i
```

```
# vsftpd.log 传输日志
Tue Jun  8 15:52:05 2021 [pid 9057] CONNECT: Client "::ffff:112.97.60.144"
Tue Jun  8 15:52:05 2021 [pid 9056] [ftp] OK LOGIN: Client "::ffff:112.97.60.144", anon password "?"
Tue Jun  8 15:52:37 2021 [pid 9058] [ftp] OK UPLOAD: Client "::ffff:112.97.60.144", "/LogFile/99099-2021_6_8_15_52_6_message.log", 8096966 bytes, 254.01Kbyte/sec
Tue Jun  8 15:53:07 2021 [pid 9058] [ftp] OK UPLOAD: Client "::ffff:112.97.60.144", "/LogFile/99099-2021_6_8_15_52_38_message.log.0", 8290312 bytes, 275.53Kbyte/sec
Tue Jun  8 15:53:57 2021 [pid 9058] [ftp] OK UPLOAD: Client "::ffff:112.97.60.144", "/LogFile/99099_BusRec_2021.6.8.15.53.8", 10485248 bytes, 232.92Kbyte/sec
Tue Jun  8 15:54:39 2021 [pid 9516] CONNECT: Client "::ffff:113.85.70.121"
Tue Jun  8 15:54:40 2021 [pid 9515] [ftp] OK LOGIN: Client "::ffff:113.85.70.121", anon password "abc@foo.bar"
Wed Jun  9 09:03:14 2021 [pid 28930] CONNECT: Client "::ffff:110.49.215.249"
Wed Jun  9 09:03:14 2021 [pid 28929] [ftp] OK LOGIN: Client "::ffff:110.49.215.249", anon password "?"
Wed Jun  9 09:03:38 2021 [pid 28931] [ftp] OK UPLOAD: Client "::ffff:110.49.215.249", "/LogFile/77077-2021_6_9_9_3_18_message.log.0", 8290343 bytes, 345.85Kbyte/sec
Wed Jun  9 09:03:49 2021 [pid 28931] [ftp] OK UPLOAD: Client "::ffff:110.49.215.249", "/LogFile/77077-2021_6_9_9_3_42_message.log", 3746585 bytes, 349.27Kbyte/sec
Wed Jun  9 09:04:23 2021 [pid 28931] [ftp] OK UPLOAD: Client "::ffff:110.49.215.249", "/LogFile/77077_BusRec_2021.6.9.9.3.52", 10485248 bytes, 347.86Kbyte/sec
```

