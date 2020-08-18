# Linux下Java的安装与配置



## yum 安装 openJDK



1.  查找java相关的列表
```
yum -y list java*
```
或
```
yum search jdk
```
2.  选择需要版本进行安装jdk, 本文以1.8为例, 安装过程中按 y
```
# 这里会自动安装1.8的最新版本
yum install java-1.8.0-openjdk.x86_64
```
3. 安装后验证
```
java -version
```
```
[root@vdo /]# java -version
openjdk version "1.8.0_232"
OpenJDK Runtime Environment (build 1.8.0_232-b09)
OpenJDK 64-Bit Server VM (build 25.232-b09, mixed mode)
```
通过 yum 安装的java, 默认路径在
```
/usr/lib/jvm
```
```
[root@vdo /]# cd /usr/lib/jvm
[root@vdo jvm]# ll
total 0
drwxr-xr-x. 3 root root 17 Dec  4 20:54 java-1.8.0-openjdk-1.8.0.232.b09-0.el7_7.x86_64
lrwxrwxrwx. 1 root root 21 Dec  4 20:54 jre -> /etc/alternatives/jre
lrwxrwxrwx. 1 root root 27 Dec  4 20:54 jre-1.8.0 -> /etc/alternatives/jre_1.8.0
lrwxrwxrwx. 1 root root 35 Dec  4 20:54 jre-1.8.0-openjdk -> /etc/alternatives/jre_1.8.0_openjdk
lrwxrwxrwx. 1 root root 51 Dec  4 20:54 jre-1.8.0-openjdk-1.8.0.232.b09-0.el7_7.x86_64 -> java-1.8.0-openjdk-1.8.0.232.b09-0.el7_7.x86_64/jre
lrwxrwxrwx. 1 root root 29 Dec  4 20:54 jre-openjdk -> /etc/alternatives/jre_openjdk
```
4. 将jdk的安装路径加到 JAVA_HOME, 这不是必须

```
# 打开 /etc/profile
vim /etc/profile
```
在文件最后加入以下内容, 按 insert 进入输入模式
```
# set java environment
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.232.b09-0.el7_7.x86_64
JRE_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.232.b09-0.el7_7.x86_64/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME JRE_HOME CLASSPATH PATH
```
按 ESC 退回命令模式, 输入 :wq 保存退出

让刚才在 /etc/profile 中的配置生效
```
# 注意 . 之后应有一个空格
. /etc/profile
```

## 官方下载安装 Java JDK

参考 https://www.cnblogs.com/wjup/p/11041274.html