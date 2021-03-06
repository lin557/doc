# Linux 命令说明



## yum

### yum出错

当在新的服务器上，使用yum命令时报出Could not retrieve mirrorlist 类似的错误。

1. 打开文件
```
vim /etc/resolv.conf
```
2. 增加以下内容
```
# generated by NetworkManager
nameserver 8.8.8.8
search localdomain
```

## 工具安装

1. rz 命令支持
```
yum -y install lrzsz
```
2. gcc 支持

```
yum -y install gcc
```

## 文件操作

### 显示文件

1. 显示所有内容
```
cat filename
```
2. 显示最后1000行
```
cat filename |tail -n 1000
```
3. 显示开始1000行
```
cat filename |head -n 1000
```
4. 从1000行开始显示，显示1000行以后的内容
```
cat filename |tail -n +1000
```
5. 查询包含指定内容的行, 本例为 kernel
```
cat filename |grep 'kernel'
```



### 编辑文件

1. 打开要编辑的文件

```
vim /data/test.sh
```



### 文件查找

1. 按文件大小查找

```
// 查找大于10M的文件
find ./ -type f -size +10M
```



### 文件上传

1. 查看rz是否安装

```
// 输入命令
rz
```
命令没安装时提示
```
[root@tg-apps2 mysql-5.7.28]# rz
-bash: rz: command not found
```

2. 安装lrzsz

```
yum -y install lrzsz
```

3. 上传文件

```
// 再次输入命令，将弹出文件选择对话框
rz
```
注：如果服务器上同名文件存在将会上传失败

### 移动文件或文件夹

~~~
# mv 源目录 新目录 如下将/home/data/mysql 移到 /home下
mv /home/data/mysql /home
~~~



## 日志查看









## 磁盘操作

### 磁盘空间

```
df -h
```

### 显示目录下文件大小

```
du -sh *
```





## 时间相关

### 查看服务器时间

```
date
```

```
[root@localhost /]# date
Wed Dec  4 20:13:12 +07 2019
```

### 查看服务器时区

```
date -R
```

```
[root@tg-apps2 ~]# date -R
Mon, 18 May 2020 10:43:47 +0800
```

### 时间同步

```
# 安装同步软件
yum install ntpdate -y
```

```
# 执行同步
ntpdate -u ntp.api.bz

# 成功提示
[root@dvr1 ~]# ntpdate -u ntp.api.bz
14 Jan 17:03:02 ntpdate[4990]: step time server 17.253.82.125 offset -25010.471297 sec
```

```
# 将时间同步到硬件 防止重启后原时间丢失
hwclock -w
```



### 查看服务器重启时间

1. 查看最后一次执行重启时间的命令

```
who –b
```
```
[root@localhost /]# who -b
         system boot  2019-09-27 17:16
```

2. 查看历史重启时间的命令

```
last reboot
```
```
reboot   system boot  3.10.0-957.27.2. Fri Sep 27 17:16 - 20:21 (68+03:04)  
reboot   system boot  3.10.0-957.27.2. Fri Sep 13 11:18 - 20:21 (82+09:02)  
reboot   system boot  3.10.0-957.27.2. Thu Sep 12 17:01 - 08:14  (15:13)    
reboot   system boot  3.10.0-957.el7.x Thu Sep 12 16:23 - 16:58  (00:35)    
reboot   system boot  3.10.0-957.el7.x Thu Sep 12 16:14 - 16:22  (00:08)    
reboot   system boot  3.10.0-957.el7.x Thu Sep 12 16:03 - 16:22  (00:19)    
reboot   system boot  3.10.0-957.el7.x Thu Sep 12 14:37 - 16:03  (01:26)    
reboot   system boot  3.10.0-957.el7.x Wed Sep 11 17:30 - 14:37  (21:06)    
reboot   system boot  3.10.0-957.el7.x Wed Sep 11 17:06 - 14:37  (21:30)    

wtmp begins Wed Sep 11 17:06:43 2019
```



## IP与端口操作



### 端口检查

1. 查看当前监听的端口
```
netstat -tlunp
```
2. 查看端口是否被占用
```
netstat -lnpt |grep 5672
```



###  查看本机IP地址

1. 旧命令

```
ip addr
```

2. Centos7使用的命令

```
ifconfig
```



## CentOS安装Windows字体

1. 进入 /usr/share/fonts 下，创建一个 winfonts目录

```
mkdir -p /usr/share/fonts/winfonts
```

2. 将 windows上的所有字体都上传到这个winfonts下，或只选出需要的字体

```
C:\Windows\Fonts
```

3. 安装字体管理软件

```
yum install -y fontconfig mkfontscale
```

4. 进入linux上的字体目录下

```
cd /usr/share/fonts/winfonts
```

5. 安装刚上传的字体

```
mkfontscale
# 有时可能需要更新字体缓存 但我没用过
fc-cache
```

6. 查看是否安装成功

```
# zh 表中文
fc-list :lang=zh
```


## 磁盘操作



### 修改挂载目录

本文将当前挂载目录 /run/media/mdstech/5e7fa244-09f5-41bd-98bd-4f0342ae2d54 修改成 /data

1. 先查看分区情况
```
df -h
```
```
[root@localhost ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   50G  8.6G   42G  18% /
devtmpfs                 5.8G     0  5.8G   0% /dev
tmpfs                    5.8G     0  5.8G   0% /dev/shm
tmpfs                    5.8G  649M  5.2G  11% /run
tmpfs                    5.8G     0  5.8G   0% /sys/fs/cgroup
/dev/sda1               1014M  232M  783M  23% /boot
/dev/mapper/centos-home   44G  137M   43G   1% /home
tmpfs                    1.2G   12K  1.2G   1% /run/user/42
tmpfs                    1.2G   44K  1.2G   1% /run/user/1000
/dev/sdb1                916G   77M  869G   1% /run/media/mdstech/5e7fa244-09f5-41bd-98bd-4f0342ae2d54
tmpfs                    1.2G     0  1.2G   0% /run/user/0
```
这里可以看出 /run/media/mdstech/5e7fa244-09f5-41bd-98bd-4f0342ae2d54 挂载在 /dev/sdb1上，本文需要换到 /data

2. 创建 /data 目录，如果没有就创建，有就跳过
```
mkdir /data
```
3. 卸载硬盘已挂载的目录
```
umount /run/media/mdstech/5e7fa244-09f5-41bd-98bd-4f0342ae2d54
```
注: 注意当前用户不要位于要卸载的目录下，不能会提示目录繁忙，如以下错误提示， 成功的没有提示的
```
[root@localhost 5e7fa244-09f5-41bd-98bd-4f0342ae2d54]# umount /run/media/mdstech/5e7fa244-09f5-41bd-98bd-4f0342ae2d54
umount: /run/media/mdstech/5e7fa244-09f5-41bd-98bd-4f0342ae2d54: target is busy.
        (In some cases useful info about processes that use
         the device is found by lsof(8) or fuser(1))
```
4. 将 /dev/sdb1 挂载到 /data, 成功无提示
```
mount /dev/sdb1 /data
```
5. 查看挂载情况
```
df -h
```
可以看到 /dev/sdb1 已挂载到 /data中
```
[root@localhost /]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   50G  8.6G   42G  18% /
devtmpfs                 5.8G     0  5.8G   0% /dev
tmpfs                    5.8G     0  5.8G   0% /dev/shm
tmpfs                    5.8G  649M  5.2G  11% /run
tmpfs                    5.8G     0  5.8G   0% /sys/fs/cgroup
/dev/sda1               1014M  232M  783M  23% /boot
/dev/mapper/centos-home   44G  137M   43G   1% /home
tmpfs                    1.2G   12K  1.2G   1% /run/user/42
tmpfs                    1.2G   44K  1.2G   1% /run/user/1000
tmpfs                    1.2G     0  1.2G   0% /run/user/0
/dev/sdb1                916G   77M  869G   1% /data
```
6. 设置开机自动挂载
```
略
```



### 腾讯云服务器磁盘扩容

腾讯提供的文档，一执行脚本就提就/dev/vdb1 in used 无法使用，这里用另外的方法

只关于MBR类型的扩容， 本文要扩屏/dev/vdb1分区， 原来是200G，现在需要扩到400G

1. 下载安装  growpart 
```
yum -y install cloud-utils-growpart
```
2. 查看所有分区
```
blkid
```
3. 分区扩容（growpart 磁盘路径 分区编号）
```
growpart /dev/vdb 1
```
4. 调整物理卷大小, 我用lvresize没成功
```
pvresize /dev/vdb1
```
执行 lsblk 查看磁盘容量 已经是400G
```
[root@VM_110_6_centos trackanalyse]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0   50G  0 disk 
└─vda1 253:1    0   50G  0 part /
vdb    253:16   0  400G  0 disk 
└─vdb1 253:17   0  400G  0 part /tgdata
```
执行 df -h
```
[root@VM_110_6_centos trackanalyse]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        50G   14G   34G  29% /
devtmpfs        7.8G  4.0K  7.8G   1% /dev
tmpfs           7.8G   24K  7.8G   1% /dev/shm
tmpfs           7.8G  748K  7.8G   1% /run
tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
tmpfs           1.6G     0  1.6G   0% /run/user/0
/dev/vdb1       197G  174G   14G  93% /tgdata
```
注: 到这里用lsblk查看到硬盘已扩容，但通过 df -h会显示容量没有变化，需要执行以下命令扩展文件系统
5. 扩展文件系统

注: 
对于 EXT 文件系统，请执行 resize2fs 命令扩容文件系统
对于 XFS 文件系统，请执行 xfs_growfs命令扩容文件系统

```
resize2fs /dev/vdb1
```
稍等一会后提示说明成功

```
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/vdb1 is mounted on /tgdata; on-line resizing required
old_desc_blocks = 13, new_desc_blocks = 25
The filesystem on /dev/vdb1 is now 104857339 blocks long.
```
执行 df -h 查看
```
[root@VM_110_6_centos trackanalyse]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        50G   13G   34G  28% /
devtmpfs        7.8G  4.0K  7.8G   1% /dev
tmpfs           7.8G   24K  7.8G   1% /dev/shm
tmpfs           7.8G  716K  7.8G   1% /run
tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
tmpfs           1.6G     0  1.6G   0% /run/user/0
/dev/vdb1       394G  174G  203G  47% /tgdata
```



### 新硬盘分区

​		parted命令可以划分单个分区大于2T的GPT格式的分区，也可以划分普通的MBR分区，fdisk命令对于大于2T的分区无法划分（大于2.2TB的存储空间用fdisk不支持，需要采用parted来分区），所以用fdisk无法看到parted划分的GPT格式的分区。 



#### fdisk分区

```
略
```

#### parted分区

1. 使用方法

```
parted [options] [device [command [options...]...]]

options
    -h  显示帮助信息
    -l  显示所有块设备上的分区

devices  对哪个块设备进行操作，如果没有指定则使用第一个块设备
    -m, --machine                   displays machine parseable output
    -s, --script                    never prompts for user intervention
    -v, --version                   displays the version
    -a, --align=[none|cyl|min|opt]  alignment for new partitions

command [options...]
    align-check TYPE N              check partition N for TYPE(min|opt) alignment
    help [COMMAND]                  print general help, or help on COMMAND
    mklabel,mktable LABEL-TYPE      create a new disklabel (partition table)
    mkpart PART-TYPE [FS-TYPE] START END     make a partition
    name NUMBER NAME                         name partition NUMBER as NAME
    print [devices|free|list,all|NUMBER]     display the partition table, available devices, free space, all found partitions, or a particular partition
    quit                            exit program
    rescue START END                rescue a lost partition near START and END
    resizepart NUMBER END           resize partition NUMBER
    rm NUMBER                       delete partition NUMBER
    select DEVICE                   choose the device to edit
    disk_set FLAG STATE             change the FLAG on selected device
    disk_toggle [FLAG]              toggle the state of FLAG on selected device
    set NUMBER FLAG STATE           change the FLAG on partition NUMBER
    toggle [NUMBER [FLAG]]          toggle the state of FLAG on partition NUMBER
    unit UNIT                       set the default unit to UNIT
    version                         显示版本号

```

2. 查看磁盘信息

```
parted -l
```
以下例子看到两个磁盘，一个42.9G, 一个21.5G（未分区）
```
[root@iZk1a23odcrt4vsq3y1k5iZ ~]# parted -l
Model: Virtio Block Device (virtblk)
Disk /dev/vda: 42.9GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End     Size    Type     File system  Flags
 1      1049kB  42.9GB  42.9GB  primary  ext4         boot


Error: /dev/vdb: unrecognised disk label
Model: Virtio Block Device (virtblk)                                      
Disk /dev/vdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags: 
```
3. 对上边的 新硬盘 /dev/vdb 进行分区
```
parted /dev/vdb
```
```
[root@iZk1a23odcrt4vsq3y1k5iZ ~]# parted /dev/vdb
GNU Parted 3.1
Using /dev/vdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) p                                                                
Error: /dev/vdb: unrecognised disk label
Model: Virtio Block Device (virtblk)                                      
Disk /dev/vdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags: 
(parted) mklabel // 创建分区表                                                         
New disk label type? gpt                                                  
(parted) mkpart primary 1 100%  // 分区
(parted) p
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 21.5GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     Flags
 1      1049kB  21.5GB  21.5GB               primary

(parted) quit 退出
```
4.  重读分区表 

```
partprobe
```

5. 创建一个xfs文件系统的分区，分区名为 /dev/vdb1
```
mkfs -t xfs /dev/vdb1
```
```
[root@iZk1a23odcrt4vsq3y1k5iZ ~]# mkfs -t xfs /dev/vdb1
meta-data=/dev/vdb1              isize=512    agcount=4, agsize=1310592 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=5242368, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

5. 挂载分区

```
// 创建挂载目录 如果存在，跳过
mkdir /data
mount /dev/vdb1 /data
```

6. 查看分区信息 blkid
```
[root@iZk1a23odcrt4vsq3y1k5iZ ~]# blkid
/dev/vda1: UUID="ed95c595-4813-480e-992b-85b1347842e8" TYPE="ext4" 
/dev/vdb: UUID="902baf9c-41e3-4190-bf93-a07a9eae6bf7" TYPE="xfs"
```
7. 开机自动挂载
```
vim /etc/fstab
// 增加以下行 保存退出
/dev/vdb1 /data xfs defaults 0 0
```