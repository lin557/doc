# Linux 命令说明



## 1. 工具安装



### 1.1 yum

当在新的服务器上，使用yum命令时报出Could not retrieve mirrorlist 类似的错误。

```sh
# 打开文件
vim /etc/resolv.conf
```

```sh
# 增加以下内容，保存退出
nameserver 8.8.8.8
search localdomain
```



### 1.2 rz

用于上传文件

```sh
yum -y install lrzsz
```

```sh
# 输入rz命令 弹出上传文件对话框
# 注：如果服务器上同名文件存在将会上传失败
rz
```



### 1.3 gcc

用于编译一些源码的程序

```sh
yum -y install gcc
```



### 1.4 telnet

telnet客户端

```sh
yum install telnet
```

```sh
# telnet ip port
telnet 192.168.0.1 9800
```



### 1.5 netstat

网络工具

```sh
yum install net-tools
```

参数说明

```
-a (all) 显示所有选项，默认不显示LISTEN相关。
-t (tcp) 仅显示tcp相关选项。
-u (udp) 仅显示udp相关选项。
-n 拒绝显示别名，能显示数字的全部转化成数字。
-l 仅列出有在 Listen (监听) 的服务状态。

-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命令。
```

```sh
# 显示监听的端口
netstat -tlnp

# 显示结果
[root@sv-cent-254-3 tomcat-gateway]# netstat -tlnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 10.1.254.3:7000         0.0.0.0:*               LISTEN      27854/./redis-serve 
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1796/master         
tcp        0      0 0.0.0.0:7100            0.0.0.0:*               LISTEN      17071/nginx: master 
tcp        0      0 0.0.0.0:38389           0.0.0.0:*               LISTEN      23972/sshd          
tcp6       0      0 127.0.0.1:8505          :::*                    LISTEN      28822/java          
tcp6       0      0 ::1:25                  :::*                    LISTEN      1796/master         
tcp6       0      0 :::17020                :::*                    LISTEN      18127/java          
tcp6       0      0 :::17180                :::*                    LISTEN      18127/java                   
tcp6       0      0 :::38389                :::*                    LISTEN      23972/sshd          
[root@sv-cent-254-3 tomcat-gateway]# 
```

```sh
# 查询指定端口
netstat -tlnp |grep 5672
```



### 1.6 iotop

```sh
yum -y install iotop
```



### 1.7 vim

```sh
yum install vim
```



### 1.8 speedtest

网络测速软件

```
# 安装 python-pip
yum install python-pip

# 安装 speedtest
pip install speedtest-cli
```

```
# 使用
speedtest-cli
```



## 2 关SELinux



### 2.1 查看状态

```
# 如果SELinux status参数为enabled即为开启状态
/usr/sbin/sestatus -v

# 也可以用这个命令检查
getenforce
```

### 2.2 关闭方法

```
# 临时关闭（不用重启机器）

# 设置SELinux 成为permissive模式
# 设置SELinux 成为enforcing模式
setenforce 0

# 修改配置文件需要重启机器
vim /etc/selinux/config

将SELINUX=enforcing改为SELINUX=disabled 重启机器
```



## 3. 信息查询



### 3.1 系统内核信息

~~~
uname -a  
~~~



### 3.2 系统版本信息 

```
cat /proc/version
```



### 3.3 系统发行版信息

```
cat /etc/redhat-release
```



### 3.4 查看cpu的信息

```
cat /proc/cpuinfo

# processor     逻辑处理器的id。
# physical id   正式的物理处理器的id。
# core id       cpu里面的每个核心的id。
# cpu cores     每个cpu物理处理器中的内核数量。
# siblings      每个cpu物理处理器中的逻辑处理器的数量。
```



### 3.5 CPU占用率

```
perf top -s comm
```



### 3.6 查看网卡信息

```
ifconfig -a
```



### 3.7 目前登入用户

- w 命令,显示目前登入系统的用户信息

```
[root@sv-cent-254-3 tomcat-gateway]# w
 14:21:10 up 3 days, 23:49,  2 users,  load average: 0.45, 0.25, 0.24
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    119.28.187.174   13:25    6.00s 14:31   0.01s w
```

- who 命令用于显示系统中有哪些使用者正在上面，显示的资料包含了使用者 ID、使用的终端机、从哪边连上来的、上线时间、呆滞时间、CPU 使用量、动作等等。

```
[root@sv-cent-254-3 tomcat-gateway]# who
root     pts/0        2021-03-31 13:25 (119.28.187.174)
```

~~~
# 踢掉在线用户下线, root用w或who查看当前登录的用户

pkill -kill -t pts/0
~~~



### 3.8 最近登录信息

用于显示用户最近登录信息

```
[root@sv-cent-254-3 tomcat-gateway]# last
root     pts/0        119.28.187.174   Wed Mar 31 13:25   still logged in   
root     pts/6        119.28.187.174   Wed Mar 31 13:18 - 13:25  (00:06)    
root     pts/6        119.28.187.174   Wed Mar 31 11:55 - 12:15  (00:19)    
root     pts/5        180.180.157.100  Wed Mar 31 11:53 - 13:22  (01:29)           
root     pts/1        119.28.187.174   Wed Mar 31 09:23 - 10:53  (01:29)    
root     pts/0        116.25.248.176   Wed Mar 31 09:20 - 13:22  (04:02)    
root     pts/2        182.232.159.170  Wed Mar 31 08:24    gone - no logout 
root     pts/1        116.25.248.176   Wed Mar 31 08:20 - 09:20  (00:59)    
root     pts/0        10.1.254.19      Wed Mar 31 08:12 - 08:26  (00:13)
```



## 4. 网络抓包

抓包命令

```
tcpdump
```



### 4.1 简易http服务

```
# 启动命令时 当前目录为http根目录 端口号8000
python -m SimpleHTTPServer 8000
```



### 4.2. tcpdump

```
# enp1s0f0  = 网卡名
# port 9010 = 只看9010端口的数据(网关)
# and host 112.96.237.2  表示只看 这个ip的数据
# -w /tmp/3002.cap   写成文件 写到 /tmp 文件名为 3002.cap
tcpdump -i enp1s0f0 port 9010 and host 112.96.237.2 -w /tmp/3002.cap
```



## 5. 磁盘操作



### 5.1 磁盘使用情况统计

```
df -h
```

```
[root@web3 /]# df -h
Filesystem                    Size  Used Avail Use% Mounted on
/dev/mapper/centos_web3-root   50G   12G   39G  23% /
devtmpfs                       32G     0   32G   0% /dev
tmpfs                          32G     0   32G   0% /dev/shm
tmpfs                          32G  505M   31G   2% /run
tmpfs                          32G     0   32G   0% /sys/fs/cgroup
/dev/sda2                    1014M  135M  880M  14% /boot
/dev/sda1                     200M  9.8M  191M   5% /boot/efi
/dev/mapper/centos_web3-home   41G   33M   41G   1% /home
/dev/sdc1                     500G   47M  500G   1% /data
tmpfs                         6.3G     0  6.3G   0% /run/user/0
```



### 5.2 显示目录下文件大小

```
du -sh *
```

```
[root@web3 ~]# cd /usr
[root@web3 usr]# du -sh *
445M	bin
0	etc
0	games
18M	include
536M	lib
313M	lib64
181M	libexec
2.9G	local
44M	sbin
587M	share
56M	src
0	tmp
```



### 5.3 文件系统类型查询

```sh
blkid
```

```
[root@web3 ~]# blkid
/dev/sdc1: UUID="5bd81640-f5ae-486f-ab4c-0e072de9dcf7" TYPE="xfs" PARTLABEL="primary" PARTUUID="d07ca82c-a12f-461a-b3a6-daa17d8718e0" 
/dev/sda1: SEC_TYPE="msdos" UUID="A998-2B2C" TYPE="vfat" PARTLABEL="EFI System Partition" PARTUUID="6c3115af-e7b9-4693-97dc-8f0f498f10d7" 
/dev/sda2: UUID="402b6f77-182a-4ffc-b353-a3aff84dbb8e" TYPE="xfs" PARTUUID="524dd9a3-8b24-476f-b490-14956bf440d5" 
/dev/sda3: UUID="NqCGy3-028j-VY3u-HAhi-SJeE-Rsz0-rU3BJ8" TYPE="LVM2_member" PARTUUID="f92781aa-0867-4203-81a9-7c5858b21dd0" 
/dev/sr0: UUID="2017-09-06-10-51-00-00" LABEL="CentOS 7 x86_64" TYPE="iso9660" PTTYPE="dos" 
/dev/mapper/centos_web3-root: UUID="bd96baaa-cec0-4dcd-a134-5a7400c63c6d" TYPE="xfs" 
/dev/mapper/centos_web3-swap: UUID="20323608-67a5-472b-8aa8-eccef2bcb898" TYPE="swap" 
```



### 5.3查看磁盘容量

```
lsblk
```

```
[root@web3 xx]# lsblk 
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0  100G  0 disk 
├─sda1                 8:1    0  200M  0 part /boot/efi
├─sda2                 8:2    0    1G  0 part /boot
└─sda3                 8:3    0 98.8G  0 part 
  ├─centos_web3-root 253:0    0   50G  0 lvm  /
  ├─centos_web3-swap 253:1    0  7.9G  0 lvm  [SWAP]
  └─centos_web3-home 253:2    0 40.9G  0 lvm  /home
sdb                    8:16   0   44G  0 disk 
sdc                    8:32   0  500G  0 disk 
└─sdc1                 8:33   0  500G  0 part /data
sr0                   11:0    1  4.2G  0 rom  
```





### 5.4 新硬盘分区

​		parted命令可以划分单个分区大于2T的GPT格式的分区，也可以划分普通的MBR分区，fdisk命令对于大于2T的分区无法划分（大于2.2TB的存储空间用fdisk不支持，需要采用parted来分区），所以用fdisk无法看到parted划分的GPT格式的分区。

​		一般使用 **parted** 命令进行分区，fdisk已过时

#### 5.4.1 操作步骤

新增一个硬盘后

- 创建磁盘分区
- 在分区上创建文件系统（格式化）
- 挂载刚创建的分区
- 配置开机自动挂载



#### 5.4.2 parted 命令说明

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

#### 5.4.3 查询磁盘信息

```sh
# 显示磁盘信息 找出要分区的新硬盘
parted -l
```

```
[root@web3 ~]# parted -l
Model: VMware Virtual disk (scsi)
Disk /dev/sda: 107GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name                  Flags
 1      1049kB  211MB   210MB   fat16        EFI System Partition  boot
 2      211MB   1285MB  1074MB  xfs
 3      1285MB  107GB   106GB                                      lvm


Error: /dev/sdb: unrecognised disk label
Model: VMware Virtual disk (scsi)                                         
Disk /dev/sdb: 47.2GB   // 这里也有一个没分区新盘 47G
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags: 

Error: /dev/sdc: unrecognised disk label
Model: VMware Virtual disk (scsi)                                         
Disk /dev/sdc: 537GB   // 这里可以看到一个新加的537G硬盘 /dev/sdc
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags: 

Model: Linux device-mapper (linear) (dm)
Disk /dev/mapper/centos_web3-home: 43.9GB
Sector size (logical/physical): 512B/512B
Partition Table: loop
Disk Flags: 

Number  Start  End     Size    File system  Flags
 1      0.00B  43.9GB  43.9GB  xfs

Model: Linux device-mapper (linear) (dm)
Disk /dev/mapper/centos_web3-root: 53.7GB
Sector size (logical/physical): 512B/512B
Partition Table: loop
Disk Flags: 

Number  Start  End     Size    File system  Flags
 1      0.00B  53.7GB  53.7GB  xfs
```



#### 5.4.4 创建磁盘分区

```sh
# 上面找到的新磁盘为 /dev/sdc
parted /dev/sdc
# 以下 (parted) 部份都需要人工输入信息进行交互
```

```
[root@web3 ~]# parted /dev/sdc
GNU Parted 3.1
Using /dev/sdc
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) p                // 这里输入 p 打印磁盘信息                                                        
Error: /dev/sdc: unrecognised disk label
Model: VMware Virtual disk (scsi)                                         
Disk /dev/sdc: 537GB
Sector size (logical/physical): 512B/512B
Partition Table: unknown  // 分区表unknown 说明还没分区过
Disk Flags:        
(parted) mklabel          // 输入 mklabel 创建磁盘标签
New disk label type? gpt  // 输入 gpt 标签类型, 一般为mbr与gpt, mbr已淘汰                                                
(parted) mkpart primary 1 100%  // 输入分区大小 1 100% 表示整个盘作为一个分区                                          
(parted) p                // 打印分区信息                                                 
Model: VMware Virtual disk (scsi)
Disk /dev/sdc: 537GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End    Size   File system  Name     Flags
 1      1049kB  537GB  537GB               primary

(parted) quit             // 创建完成 退出                                                
Information: You may need to update /etc/fstab.  // 这个提示配置开机挂载后面处理
```

```sh
# 更新分区表
partprobe
```

```sh
# 可以再次使用 parted -l 查看分区信息
[root@web3 ~]# parted -l
Model: VMware Virtual disk (scsi)
Disk /dev/sda: 107GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name                  Flags
 1      1049kB  211MB   210MB   fat16        EFI System Partition  boot
 2      211MB   1285MB  1074MB  xfs
 3      1285MB  107GB   106GB                                      lvm


Error: /dev/sdb: unrecognised disk label
Model: VMware Virtual disk (scsi)                                         
Disk /dev/sdb: 47.2GB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags: 

Model: VMware Virtual disk (scsi)  // 这里说明有了新分区了
Disk /dev/sdc: 537GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End    Size   File system  Name     Flags
 1      1049kB  537GB  537GB               primary
```



#### 5.4.5 创建文件系统

文件系统说明, @see https://blog.csdn.net/kwame211/article/details/91043893

常用: ext3, ext4, xfs, 这里用xfs

```sh
# xfs=文件系统, /dev/sdc=表示磁盘 sdc1 sdc2 sdc3=表示分区, 目前只有一个分区, 所以是sdc1
mkfs -t xfs /dev/sdc1
```

```sh
[root@web3 ~]# mkfs -t xfs /dev/sdc1
meta-data=/dev/sdc1              isize=512    agcount=4, agsize=32767872 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=131071488, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=63999, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
# 等上几秒就可以
```

```sh
# 也可以创建 ext4 文件系统
mkfs -t ext4 /dev/sdc1

# 如果想将ext4 转为 xfs可以用以下命令
# umount /data 如果有挂载，先取消挂载 这里假设 /data是挂载在分区上的目录
mkfs.xfs -f /dev/sdc1
```



#### 5.4.6 挂载新分区

```sh
# 创建挂载目录 如果存在，跳过
mkdir /data
mount /dev/sdc1 /data
```



#### 5.4.7 配置开机自动挂载

```sh
vim /etc/fstab
# 增加以下行 保存退出
/dev/sdc1 /data xfs defaults 0 0
```



### 5.5 磁盘扩容





## 6. 时间相关



### 6.1 查看时间

```sh
date
```

```sh
[root@localhost /]# date
Wed Dec  4 20:13:12 +07 2019
```



### 6.2 查看时区

```sh
date -R
```

```sh
[root@tg-apps2 ~]# date -R
Mon, 18 May 2020 10:43:47 +0800
```



### 6.3 时间同步

```sh
# 安装同步软件
yum install ntpdate -y
```

```sh
# 执行同步
# ntpdate time.nist.gov
ntpdate -u ntp.api.bz

# 成功提示
[root@dvr1 ~]# ntpdate -u ntp.api.bz
14 Jan 17:03:02 ntpdate[4990]: step time server 17.253.82.125 offset -25010.471297 sec
```

```sh
# 将时间同步到硬件 防止重启后原时间丢失
hwclock -w
```



### 6.4 查看重启时间

```sh
# 查看最后一次执行重启时间的命令
who –b
```

```sh
[root@localhost /]# who -b
         system boot  2019-09-27 17:16
```

```sh
# 查看历史重启时间的命令
last reboot
```

```
reboot   system boot  3.10.0-957.27.2. Fri Sep 27 17:16 - 20:21 (68+03:04)  
reboot   system boot  3.10.0-957.27.2. Fri Sep 13 11:18 - 20:21 (82+09:02)  
reboot   system boot  3.10.0-957.27.2. Thu Sep 12 17:01 - 08:14  (15:13)    
reboot   system boot  3.10.0-957.el7.x Thu Sep 12 16:23 - 16:58  (00:35)    
reboot   system boot  3.10.0-957.el7.x Thu Sep 12 16:14 - 16:22  (00:08)      

wtmp begins Wed Sep 11 17:06:43 2019
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



### 移动文件或文件夹

~~~
# mv 源目录 新目录 如下将/home/data/mysql 移到 /home下
mv /home/data/mysql /home
~~~



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



