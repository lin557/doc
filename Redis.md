# Redis 6.0.x 在 linux 下的安装与配置

## vi/vim

1. 如果没有安装vim, 可以先安装
```
yum install vim -y
```
2. 不想使用 vim, 也可以用系统自带的 vi

## 安装

1. 上官网或中文网下载最新稳定版本(http://www.redis.cn/)，当前文档使用的版本是 6.0.5
2. 下载后（文件名 redis-6.0.5.tar.gz），上传到linux服务器上，或者直接在linux服务器上直接下载,下载命令如下，文档中下载到 /usr/local 目录下
```
# 没装wget 可以用 yum install wget 安装
cd /usr/local
wget http://download.redis.io/releases/redis-6.0.5.tar.gz
```
3. 解压文件到 /usr/local/redis-6.0.5
```
tar xzf redis-6.0.5.tar.gz
```
4. 进入目录
```
cd /usr/local/redis-6.0.5
```
5. 下载的是源码版本, 需要编译后才能使用。编译需要gcc, 如果电脑没有安装gcc, 需先安装, 有就跳过
```
# 安装gcc命令
yum install gcc
```
6. 编译与安装redis, 编译大约需要一至两分钟
```
# 编译命令
make
```
```
# 6.0.x版本可能会有以下报错, 5.0.x不会。原因是gcc版本问题, 6.0.x需要gcc 5.3以上
...
server.c:2872:11: error: ‘struct redisServer’ has no member named ‘repl_good_slaves_count’
     server.repl_good_slaves_count = 0;
           ^
server.c:2884:27: error: ‘struct redisServer’ has no member named ‘ipfd_count’
     for (j = 0; j < server.ipfd_count; j++) {
                           ^
server.c:2885:48: error: ‘struct redisServer’ has no member named ‘ipfd’
         if (aeCreateFileEvent(server.el, server.ipfd[j], AE_READABLE,
                                                ^
server.c:2892:27: error: ‘struct redisServer’ has no member named ‘tlsfd_count’
     for (j = 0; j < server.tlsfd_count; j++) {
                           ^
server.c:2893:48: error: ‘struct redisServer’ has no member named ‘tlsfd’
         if (aeCreateFileEvent(server.el, server.tlsfd[j], AE_READABLE,
                                                ^
server.c:2900:15: error: ‘struct redisServer’ has no member named ‘sofd’
     if (server.sofd > 0 && aeCreateFileEvent(server.el,server.sofd,AE_READABLE,
               ^
server.c:2900:62: error: ‘struct redisServer’ has no member named ‘sofd’
     if (server.sofd > 0 && aeCreateFileEvent(server.el,server.sofd,AE_READABLE,
                                                              ^
server.c:2906:44: error: ‘struct redisServer’ has no member named ‘module_blocked_pipe’
     if (aeCreateFileEvent(server.el, server.module_blocked_pipe[0], AE_READABLE,
                                            ^
server.c:2919:15: error: ‘struct redisServer’ has no member named ‘aof_state’
     if (server.aof_state == AOF_ON) {
...
```

检查并更新gcc版本

```
# 检查gcc版本 centos7默认是4.8.5 
# 命令 gcc -v

[root@tg-apps2 redis-6.0.5]# gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-redhat-linux/4.8.5/lto-wrapper
Target: x86_64-redhat-linux
Configured with: ../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --with-linker-hash-style=gnu --enable-languages=c,c++,objc,obj-c++,java,fortran,ada,go,lto --enable-plugin --enable-initfini-array --disable-libgcj --with-isl=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/isl-install --with-cloog=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/cloog-install --enable-gnu-indirect-function --with-tune=generic --with-arch_32=x86-64 --build=x86_64-redhat-linux
Thread model: posix
gcc version 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
```

```
# 更新gcc 升级到9.x

# 安装
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
# 启用
scl enable devtoolset-9 bash
```

再次执行编译

```
# 编译命令
make
```



如果正常会出现以下提示

```
Hint: It's a good idea to run 'make test' ;)

make[1]: Leaving directory `/usr/local/redis-6.0.5/src'
```
7. 运行 make test 以验证是否成功。如果有错误提示，要据提示进行操作make test
```
make test
```
如果出现以下提示
```
You need tcl 8.5 or newer in order to run the Redis test
make: ***[test] Error_1
```
需要安装tcl
```
# 安装命令
yum install tcl
```
如果出现以下提示
```
# 机器配置太低提示 不影响
!!! WARNING The following tests failed:

*** [err]: Active defrag big keys in tests/unit/memefficiency.tcl
Expected condition '$max_latency <= 120' to be true (231 <= 120)
```
```
!!! WARNING The following tests failed:

*** [err]: SLOWLOG - can be disabled in tests/unit/slowlog.tcl
Expected '1' to be equal to '0'
Cleanup: may take some time... OK
make[1]: *** [test] Error 1
make[1]: Leaving directory `/usr/local/redis-6.0.5/src'
make: *** [test] Error 2
```
都是WARNING可以不管 不影响使用

## 配置

1. 进入安装目录, 文档中使用的目录是 /usr/local/redis-6.0.5
```
cd /usr/local/redis-6.0.5
```

2. 找到配置文件进行编辑
```
vim redis.conf
```

3. 可以直接测试启动, 启动后按下 ctrl+c 可以退出
```
./src/redis-server redis.conf
```

4. 开启外网访问功能(如果开启了防火墙, 需要将端口加入到防火墙的规则中, 这里不做防火墙配置的说明)

默认配置下, 只能本机访问, 需要修改配置, vim打开配置，并找到 bind 127.0.0.1, 在前面加上 # 号改为
```
#bind 127.0.0.1
```
找到 protected-mode yes 改为
```
protected-mode no
```
重启redis, 外网就可以访问了

5. 修改默认端口, 默认端口一般不使用, 容易被攻击
```
vim redis.conf
```
找到port 6379, 将6379替换成自己的端口, 数据太多, 可以按 page down快速翻页
```
# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379
```
按 Insert 将vim切换到编辑模式 将6379改成7000, 按Esc退出编辑, 按:wq 退出并保存
再次执行 "3" 中的命令启动， 看到以下提示说明修改成功，如果端口被占用是有错误提示的。

```
Redis 6.0.5 (00000000/0) 64 bit
Running in standalone mode
Port: 7000
```

6. 给redis增加密码, 默认是没有密码的，对外开放很容易被入侵(笔者的两台redis服务器因没设密码, 被人种了好多挖矿程序)
```
vim redis.conf
```
找到 # requirepass foobared (大约在36%的位置), 可以用vim查找命令 /# requirepass查找
```
# requirepass foobared
```
把requirepass前面的#和空格去掉，把 foobared  改为自己想写的密码比如123, :wq 保存退出
```
// 用redis-cli访问, 验证密码
./src/redis-cli -h 127.0.0.1 -p 7000
// 测试 keys 命令
127.0.0.1:7000> keys *
// 出现以下提示说明需要密码
(error) NOAUTH Authentication required.
// 利用auth指令输入密码
127.0.0.1:7000> auth 123
// 说明成功 密码错误会提示 (error) ERR invalid password
OK
// 退出
127.0.0.1:7000> quit
```

7. 开启AOF持久化, redis重启后会优先检查AOF, 然后才是RDB. 目前是AOF是Redis持久化的主流方式。redis中数据比较多时AOF更有性能优势, 数据量小时 RDB 就可以
使用vim打开配置, 并找到 appendonly no (50%的位置)改为 yes
```
appendonly yes
```
:wq 退出并保存

## 集群

1. 5.0的集群不再需要安装ruby

2. 在单台机器上没必要使用集群方案

3. 以后再补

## 启动与关闭

###  启动

1. 进入安装目录, 文档中使用的目录是 /usr/local/redis-6.0.5
```
cd /usr/local/redis-6.0.5
```

2. 启动

启动后在前台运行
```
./src/redis-server redis.conf
```

启动后在后台运行
vim 打开 redis.conf 找到daemonize的值改为yes
```
./src/redis-server redis.conf
```
或直接
```
./src/redis-server redis.conf &
```

3. 启动后的警告
```
1452:M 01 Nov 2019 14:31:41.938 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
```
大概意思对一个高负载的环境来说tcp设置128这个值，太小了, 修改方法如下:
临时办法, 重启后失效
```
echo 511 > /proc/sys/net/core/somaxconn
```
永久解决，打开 /etc/sysctl.conf
```
vim /etc/sysctl.conf
```
在最后添加 net.core.somaxconn= 1024 保存退出
```
net.core.somaxconn = 1024
```
执行以下命令生效
```
sysctl -p
```

```
19114:M 01 Nov 2019 14:56:00.066 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
```

同上修改，在sysctl.conf添加一笔vm.overcommit_memory=1, 保存退出
```
vm.overcommit_memory = 1
```
```
# 执行以下命令生效
sysctl -p
```
```
22316:M 01 Nov 2019 15:00:17.844 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
```
禁用linux的THP(Transparent Huge Pages)功能
Centos7时默认启用，用来提升内存性能, Oracle、MariaDB、MongoDB、VoltDB、Redis等等在使用时要求禁用
```
# 查看是否启用
[root@VM_157_185_centos redis-6.0.5]# cat /sys/kernel/mm/transparent_hugepage/defrag
[always] madvise never
[root@VM_157_185_centos redis-6.0.5]# cat /sys/kernel/mm/transparent_hugepage/enabled
[always] madvise never
```
禁用方法

```
# vim打开文件
vim /etc/rc.d/rc.local
```

```
# 增加以下内容
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
echo never > /sys/kernel/mm/transparent_hugepage/defrag
fi
```
按 ESC, 输入:wq 保存并退出
```
# rc.local添加可执行权限
chmod +x /etc/rc.d/rc.local
```
最后重启系统生效(没找到不重启的办法)



### 旧版集群启动

```
redis-server /usr/local/redis-3.2.6/redis_cluster/7000/redis.conf

redis-server /usr/local/redis-3.2.6/redis_cluster/7001/redis.conf

redis-server /usr/local/redis-3.2.6/redis_cluster/7002/redis.conf

redis-server /usr/local/redis-3.2.6/redis_cluster/7003/redis.conf

redis-server /usr/local/redis-3.2.6/redis_cluster/7004/redis.conf

redis-server /usr/local/redis-3.2.6/redis_cluster/7005/redis.conf
```



### 关闭

关闭redis, 不要直接kill -9 pid, 以保证数据的安全与完整。 
    
1. 如果redis在前台运行, 可以按下ctrl + c 退出

2. 使用命令行退出
```
// 进入目录
/usr/local/redis-6.0.5
// 用redis-cli访问
./src/redis-cli -h 127.0.0.1 -p 7000
// 输入密码 如果没有密码 可以跳过这一步
auth 123
// 退出 并 保存持久化
shutdown save
```

## 数据复制

### 建立复制

1. 进入redis的src目录
```
cd /usr/local/redis-6.0.5/src
```
2. 打开redis-cli
```
// 本地的redis的ip与端口
redis-cli -h 10.144.110.6 -p 7000
```
```
// 如果配置了密码 需要输入密码 123456 表示密码
auth 123456
```
4. 查看redis信息
```
info replication
```
```
# Replication
role:master  // 注意这里是master
connected_slaves:0
master_replid:f70aafe7eed40dd444deeae64aad7d80332e3332
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```
3. 将本地redis设成从服务
```
// 远程redis的ip与端口
// 如果远程是集群 只能复制其中一个节点的数据, 没找到复制所有节点的方法
slaveof 10.144.110.6 7000
```
输入info replication命令查看信息
```
# Replication
role:slave // 这里变slave表示命令成功
master_host:10.144.110.6
master_port:7000
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_repl_offset:1074862
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:fa6094ce143641cd8b4ec84941f63effc862441c
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1074862
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:26287
repl_backlog_histlen:1048576
```
注：到此复制功能成功，远程服务器执行什么操作，本地服务器都会同步。

4. 断开复制
```
slaveof no one
```

### 数据同步
1. 启动redis-cli 方法参考上边数据复制1 2 3点
2. 将本地redis设成从服务
```
// 远程redis的ip与端口
// 如果远程是集群 只能复制其中一个节点的数据, 没找到复制所有节点的方法
slaveof 10.144.110.6 7000
```
3. 先去远程redis上执行redis-cli,执行以下命令获取run_id与偏移量
```
// run_id
info server
```
```
# Server
redis_version:3.2.6
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:e671808ab684834a
redis_mode:cluster
os:Linux 3.10.0-693.11.6.el7.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
gcc_version:4.8.5
process_id:11148
// 这个就是
run_id:fa6094ce143641cd8b4ec84941f63effc862441c
tcp_port:7000
uptime_in_seconds:45205001
uptime_in_days:523
hz:10
lru_clock:14383470
executable:/usr/local/redis-3.2.6/src/redis-server
config_file:/usr/local/redis-3.2.6/redis_cluster/7000/redis.conf
```
```
// 偏移量
info replication
```
```
# Replication
role:slave
master_host:10.144.110.6
master_port:7003
master_link_status:up
master_last_io_seconds_ago:0
master_sync_in_progress:0
slave_repl_offset:175914919025
slave_priority:100
slave_read_only:1
connected_slaves:1
slave0:ip=10.144.110.6,port=7000,state=online,offset=3627985,lag=1
master_repl_offset:3630882
repl_backlog_active:1
repl_backlog_size:1048576
// 这个就是偏移量
repl_backlog_first_byte_offset:2582307
repl_backlog_histlen:1048576
```
4. 利用psync命令同步

全量复制
```
// run_id=? 偏移量-1 这里-1时，如果远程redis在写入数据，这里也会跟着一直同步
psync ? -1
```
部份复制
```
// 指定运行id与偏移量
psync fa6094ce143641cd8b4ec84941f63effc862441c 2582307
```

