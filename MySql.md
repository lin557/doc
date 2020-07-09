# MySql

## 安装

### 下载mysql

1. 官网下载 5.7.X
```
https://dev.mysql.com/downloads/mysql/5.7.html
centos 对应的是 Red Hat Enterprise Linux / Oracle Linux
7 对应 Red Hat Enterprise Linux 7 / Oracle Linux 7 (x86, 64-bit)
也可以下载 Linux - Generic 版本
```
2. 利用wget 下载
```
cd /data
// 创建一个目录 mysql目录
mkdir /data/mysql
// 进入 /data/mysql目录
cd ./mysql
// 下载文件
wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.28-linux-glibc2.12-x86_64.tar.gz
或
wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.28-el7-x86_64.tar.gz
```

### 解压到 /data/mysql

1. 解压
```
tar xzf mysql-5.7.28-linux-glibc2.12-x86_64.tar.gz
```

2. 文件夹改名为mysql-5.7.28
```
mv mysql-5.7.28-linux-glibc2.12-x86_64 mysql-5.7.28
```

### 配置

1. 进入mysql目录
```
cd /data/mysql/mysql-5.7.28
```
2. 创建my.cnf文件
```
vim my.cnf
```
// 输入以下内容
```
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html
# *** DO NOT EDIT THIS FILE. It's a template which will be copied to the
# *** default location during install, and will be replaced if you
# *** upgrade to a newer version of MySQL.

[client]
port = 3306
socket = /tmp/mysql.sock

[mysqld]

# 这里一定要配用户 否则启动不了，报错Starting MySQL…The server quit without updating PID file
user = root

# pid文件
pid-file = /data/mysql/mysql-5.7.28/mysqld.pid

# 不区分大小写
lower_case_table_names = 1

# Mysql服务的唯一编号 每个mysql服务Id需唯一
server-id = 1

# 服务端口号 默认3306
port = 3306

# mysql安装根目录
basedir = /data/mysql/mysql-5.7.28

# mysql数据文件所在位置
datadir = /data/mysql/mysql-5.7.28/data

# 临时目录 比如load data infile会用到
tmpdir  = /tmp

# 设置socke文件所在目录
socket  = /tmp/mysql.sock

character-set-server=utf8
 
collation-server=utf8_general_ci

# 时区 00:00
default-time_zone = '+00:00'

# 只能用IP地址检查客户端的登录，不用主机名
skip_name_resolve = 1

# SQL数据包发送的大小，如果有BLOB对象建议修改成1G
max_allowed_packet = 10M

# MySQL连接闲置超过一定时间后(单位：秒)将会被强行关闭
# MySQL默认的wait_timeout  值为8个小时, interactive_timeout参数需要同时配置才能生效
interactive_timeout = 1800
wait_timeout = 1800

# 内部内存临时表的最大值 ，设置成128M。
# 比如大数据量的group by ,order by时可能用到临时表，
# 超过了这个值将写入磁盘，系统IO压力增大
tmp_table_size = 134217728
max_heap_table_size = 134217728

# 关闭关于 TIMESTAMP with implicit DEFAULT value is deprecated. 的警告
explicit_defaults_for_timestamp = true

# 开启事件功能
event_scheduler= ON

#日志设置

# 数据库错误日志文件
log_error = /data/mysql/mysql-5.7.28/logs/error.log

# 慢查询sql日志设置
slow_query_log = 1
slow_query_log_file = /data/mysql/mysql-5.7.28/logs/slow.log

# 检查未使用到索引的sql
log_queries_not_using_indexes = 1

# 针对log_queries_not_using_indexes开启后，记录慢sql的频次、每分钟记录的条数
log_throttle_queries_not_using_indexes = 5

# 作为从库时生效,从库复制中如何有慢sql也将被记录
#log_slow_slave_statements = 1

# 慢查询执行的秒数，必须达到此值可被记录
#long_query_time = 8

# 检索的行数必须达到此值才可被记为慢查询
#min_examined_row_limit = 100

# mysql binlog日志文件保存的过期时间，过期后自动删除
#expire_logs_days = 5

# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin

# These are commonly set, remove the # and set as required.
# basedir = .....
# datadir = .....
# port = .....
# server_id = .....
# socket = .....

# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M 

sql_mode=NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```

3. 进入 support-files 目录
```
cd ./support-files
```
4. 配置 mysql.server 文件
```
vim mysql.server
```
```
# 找到
basedir=
datadir=
# 修改为
basedir=/data/mysql/mysql-5.7.28
datadir=/data/mysql/mysql-5.7.28/data
```
按ESC, 输入:wq保存退出

5. 添加环境变量
```
# 打开文件
vim /etc/profile
```
输入以下内容，注意添加在export之前，且把MYSQL_HOME变量也加在export 后面
```
MYSQL_HOME=/data/mysql/mysql-5.7.28

PATH=$PATH:$MYSQL_HOME/bin
```
PATH有别的内容可以用冒号分隔，如以下格式

```
PATH=$PATH:$JAVA_HOME:$MYSQL_HOME/bin
```

让配置生效

```
source /etc/profile
或
. /etc/profile
```

6. 初始化数据库，得到初始随机密码

```
# 回到安装目录
cd /data/mysql/mysql-5.7.28
```
```
./bin/mysqld --user=root --basedir=/data/mysql/mysql-5.7.28 --datadir=/data/mysql/mysql-5.7.28/data --initialize
```
初始化时，如果出现以下错误
```
[root@vm mysql-5.7.28]# ./bin/mysqld --user=root --basedir=/data/mysql/mysql-5.7.28 --datadir=/data/mysql/mysql-5.7.28/data --initialize
./bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
[root@iZk1a23odcrt4vsq3y1k5iZ mysql-5.7.28]# /data/mysql/mysql-5.7.28/bin/mysqld --user=root --basedir=/data/mysql/mysql-5.7.28 --datadir=/data/mysql/mysql-5.7.28/data --initialize
/data/mysql/mysql-5.7.28/bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
```
执行以下命令安装 libaio就可以
```
yum install -y libaio
如果安装了 libaio还报错 再装下边这个
yum -y install numactl
```

执行成功
```
[root@iZk1a23odcrt4vsq3y1k5iZ mysql-5.7.28]# ./bin/mysqld --user=root --basedir=/data/mysql/mysql-5.7.28 --datadir=/data/mysql/mysql-5.7.28/data --initialize 
2019-12-25T07:32:46.578857Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2019-12-25T07:32:47.435848Z 0 [Warning] InnoDB: New log files created, LSN=45790
2019-12-25T07:32:47.547091Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2019-12-25T07:32:47.607376Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: bf8b66bc-26e8-11ea-bb5f-00163e016e15.
2019-12-25T07:32:47.608995Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2019-12-25T07:32:48.259871Z 0 [Warning] CA certificate ca.pem is self signed.
2019-12-25T07:32:48.366286Z 1 [Note] A temporary password is generated for root@localhost: M*(gwtohC1_r  // 这个就是初始密码
```

7. 启动服务

```
# 启动前 在/var/log 下创建 mariadb文件夹，并在文件夹中创建 mariadb.log 文件
# 否则启动会报错
cd /var/log
mkdir /var/log/mariadb

# 创建 logs 目录 并创建 slow.log文件
cd /data/mysql/mysql-5.7.28/
mkdir /data/mysql/mysql-5.7.28/logs
```
```
# 失败提示
[root@tg-apps2 mysql-5.7.28]# ./support-files/mysql.server start
Starting MySQL.Logging to '/var/log/mariadb/mariadb.log'.
The server quit without updating PID file (/data/mysql/mysq[FAILED]/mysqld.pid).
```

```
# 启动
./support-files/mysql.server start
```
```
# 成功提示
[root@tg-apps2 mysql-5.7.28]# ./support-files/mysql.server start
Starting MySQL. 
```

8. 以初始密码登陆, 并修改密码
```
mysql -u root -p
```
```
# 如果服务没启动会出现以下提示
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)

# 如果没配置环境变量会出现以下提示
[root@tg-apps2 mysql-5.7.28]# mysql -u root -p
-bash: mysql: command not found
```
输入初始密码(xshell中可以复制粘贴)后，通过以下命令修改密码
```
# new password 为新密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```

以下不是必须
a. 刚改完码后不记得了，可以到mysql目录下，找到data，重命名为data1或其他名字，然后重新执行初始化操作。记得改名之前需要停止mysql服务

b. 运行期改密码
```
# 输入
mysql> use mysql;
Database changed

# 修改root密码
mysql> update mysql.user set authentication_string=password('abc') where user='root';

# 成功提示
Query OK, 1 row affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 1

# 密码生效
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

```

9. 开启远程访问
在mysql的命令行中输入以下指令
```
# 执行
use mysql; // 回车
```
```
# 提示
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

```
# 执行
select host,user from user; // 回车
```
```
# 提示
+-----------+---------------+
| host      | user          |
+-----------+---------------+
| localhost | mysql.session |
| localhost | mysql.sys     |
| localhost | root          |
+-----------+---------------+
3 rows in set (0.00 sec)
```

```
# 执行
update user set host='%' where user='root';
```
```
# 提示
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

```
# 最后执行
flush privileges;
```



## 启动与停止

./support-files/ 目录在mysql的安装目录下 本文在 /data/mysql/mysql-5.7.28/ 下

### 启动

1. 利用support-files中的mysql.server

```
./support-files/mysql.server start
```



### 停止

注意不要强行kill pid进程，否则很有可能造成表损坏

1. 利用support-files中的mysql.server
```
./support-files/mysql.server stop
```

### 重启

1. 利用support-files中的mysql.server
```
./support-files/mysql.server restart
```

## 常用命令



### 查看进程列表

```sql
// 只列出当前100条
SHOW processlist;
```

```sql
// 列出所有
SHOW FULL processlist;
```

### 清空表

```sql
// 速度快 不产生日志 重置自增长id 不可恢复
TRUNCATE TABLE 表名;
```
```sql
// 速度慢 可恢复 不重置自增长id
DELETE FROM 表名;
```


## 分区表

- 一个表最多支持1024个分区
- 如果分区字段中有主键或唯一索引的列，那么所有主键列和唯一索引列都必须包含进来
- 分区表无法使用外键约束
- 所有分区必须使用相同的存储引擎

### 创建分区表

注：用于分区的字段必须为主键, 用时间做主键时需要加上id做联合主键，因为时间容易重复。分区参数中 “LESS THAN” 是小于的意思

1. 按年份分区

```sql
// 按年份分区
CREATE TABLE `表名` (
    `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
    `data_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP
    PRIMARY KEY (`id`, `data_time`)
) ENGINE = INNODB PARTITION BY RANGE (YEAR(data_time))(
    PARTITION `p_2018` VALUES LESS THAN (2019),
    PARTITION `p_2019` VALUES LESS THAN (2020)
);
```
2. 按月份分区
```sql
// 按月份分区
CREATE TABLE `表名` (
    `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
    `data_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP
    PRIMARY KEY (`id`, `data_time`)
) ENGINE = INNODB PARTITION BY RANGE (TO_DAYS(data_time))(
    PARTITION `p_201812` VALUES	LESS THAN (TO_DAYS('2019-01-01')),
    PARTITION `p_201901` VALUES LESS THAN (TO_DAYS('2019-02-01')),
    PARTITION `p_201902` VALUES LESS THAN (TO_DAYS('2019-03-01'))
);
```



### 普通表转为分区表

```
略
```



###  增加分区

以按月分区为例

```sql
ALTER TABLE `表名`
ADD PARTITION (
    PARTITION `p_201903` VALUES LESS THAN (TO_DAYS('2019-04-01')),
    PARTITION `p_201904` VALUES LESS THAN (TO_DAYS('2019-05-01'))
);
```



###  删除分区

注: 删除多个分区可以在多个分区名间用逗号(,) 分开

```sql
ALTER TABLE `表名` DROP PARTITION `分区名1`,`分区名2`;
```



### 自动创建新分区

1. 创建存储过程, 每次执行检测下个月的分区是否创建, 如果不存在则创建

```sql
DELIMITER $$
# 该表所在数据库名称 如gateway
USE `gateway`$$
DROP PROCEDURE IF EXISTS `p_create_partition_by_month`$$
CREATE PROCEDURE `p_create_partition_by_month`(IN_SCHEMANAME VARCHAR(64), IN_TABLENAME VARCHAR(64))
BEGIN
    # 需要创建的分区的个数
    DECLARE ROWS_CNT INT UNSIGNED;
    # 目前日期
    DECLARE TARGET_DATE TIMESTAMP;
    # 分区的名称，格式为p_201806
    DECLARE PARTITIONNAME VARCHAR(8);
    # 当前分区名称的分区值上限，即为 PARTITIONNAME + 1
    DECLARE PARTITION_ADD_MONTH VARCHAR(10);

    # 当前时间 + 一个月 表下个月
    SET TARGET_DATE = NOW() + INTERVAL 1 MONTH;
    # 需要新建的分区名 格式为p_201806
    SET PARTITIONNAME = DATE_FORMAT( TARGET_DATE, 'p_%Y%m' );
    
    # 新分区参数需要再加一个月 如 p_201806 对应的是 2018-07-01 表示小于2018-07-01的数据存放p_201806
    SET TARGET_DATE = TARGET_DATE + INTERVAL 1 MONTH;
    # LESS THAN 后的参数, 格式为2018-07-01
    SET PARTITION_ADD_MONTH = DATE_FORMAT( TARGET_DATE, '%Y-%m-01' );

    # 检查要建的分区是否存在
    SELECT COUNT(*) INTO ROWS_CNT FROM 
        information_schema.partitions
    WHERE table_schema = IN_SCHEMANAME AND table_name = IN_TABLENAME AND partition_name = PARTITIONNAME;

    IF ROWS_CNT = 0 THEN
        # 要建的分区不存在就创建
        SET @SQL = CONCAT( 'ALTER TABLE `', IN_SCHEMANAME, '`.`', IN_TABLENAME, '`',
            ' ADD PARTITION (PARTITION `', PARTITIONNAME, "` VALUES LESS THAN (TO_DAYS('",
            PARTITION_ADD_MONTH ,"')));" );
        SELECT @SQL;
        PREPARE STMT FROM @SQL;
        EXECUTE STMT;
        DEALLOCATE PREPARE STMT;
    ELSE
        SELECT CONCAT("partition `", PARTITIONNAME, "` for table `",IN_SCHEMANAME, ".", IN_TABLENAME, "` already exists") AS result;
    END IF;
END$$
DELIMITER ;
```

2. 数据库定时任务(每天)
```sql
DELIMITER $$
# 该表所在的数据库名称
USE `gateway`$$
CREATE EVENT IF NOT EXISTS `e_daily_generate_partition`
# 执行周期，还有时、天、月等等
ON SCHEDULE EVERY 1 day
STARTS '2019-12-01 00:00:00'
ON COMPLETION PRESERVE
ENABLE
COMMENT 'Creating partitions'
DO BEGIN
    # 调用刚才创建的存储过程，第一个参数是数据库名称，第二个参数是表名称
    CALL p_create_partition_by_month('gateway','terminal_position');
END$$
DELIMITER ;
```

3. 定时任务 如果没有执行，请检查MySql是否开启了event(默认是关闭的) 
```
[mysqld]
event_scheduler=ON
```



### 查看分区表数据信息

```sql
SELECT
    TABLE_SCHEMA,
    TABLE_NAME,
    PARTITION_NAME,
    TABLE_ROWS
FROM
    information_schema.PARTITIONS
WHERE
    TABLE_NAME = '表名'
```



## 参数配置

注: 以下通用 set global xxx 配置的参数, 在mysql重启后会失效

### 调整执行sql语句的长度

1. 查询默认值 默认为 1048576 也就是 1M

```sql
show VARIABLES like '%max_allowed_packet%';
```
2. 修改默认值大小 如 10M, 

```
// 注意修改后需要断开session, 再重新建立连接session查询才能看到修改后的效果
// 对当前会话无效
set global max_allowed_packet = 1*1024*1024*10
```



###  开启事件功能

1. 检测事件是否开启, event_scheduler: ON/OFF

```sql
show variables like 'event_scheduler';
```
2. 开启事件

```sql
set global event_scheduler = on;
```

3. 永久开启, my.ini/my.conf中修改, 需重启mysql
```
[mysqld]
event_scheduler=ON
```



### 设置时区

1. 查看时区，默认跟系统同一时区

```
SHOW VARIABLES LIKE "%time_zone%";
```

2. 修改时区，马上生效，但重启后会失效

```
// 设为utc时区
SET GLOBAL time_zone = '+00:00';
// 刷新一下
FLUSH PRIVILEGES;
```


3. 永久生效

```
// 打开my.conf, 在 [mysqld]节点下增加以下内容，重启生效
# 时区 00:00
default-time_zone = '+00:00'
```
