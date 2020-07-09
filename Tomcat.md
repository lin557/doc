# Tomcat 8.5.x 安装与配置

## 安装

1. 下载

```
进入官方下载页,并下载对应版本, linux版后缀为 tar.gz
https://tomcat.apache.org/download-80.cgi
```
```
// 上传到 /usr/local 这个目录下
可使用xftp软件上传，或用rz命令上传
```

2. 解压

```
// 本文使用的版本为 apache-tomcat-8.5.31.tar.gz
cd /usr/local
tar xzf apache-tomcat-8.5.31.tar.gz
```

## 配置

1. 进入目录

```
cd /usr/local/apache-tomcat-8.5.31
```

2. 配置server.xml

```
vim ./conf/server.xml
```
修改默认端口 启用压缩 配置http头大小等

```
# 找到以下内容 40%的位置
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
```
# 修改为
    <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               maxPostSize="-1"
               maxHttpHeaderSize="102400"
               maxThreads="500"
               compression="on"
               compressionMinSize="2048"
               noCompressionUserAgents="gozilla, traviata"
               compressableMimeType="text/html,text/xml,text/javascript,application/javascript,text/css,text/plain,text/json,application/json"
               redirectPort="8443" />
```

增加虚拟目录

```
// 找到 90%位置
    <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
```
```
// 在底下增加一行(相当于访问根目录时跳转到 /pass 工程, 这里不是必需)
        <Context path="" docBase="pass" />
```
```
// 在底下增加一行(这里表示访问虚拟路径/files 相当于访问实际路径 /data/files)
        <Context docBase="/data/files" path="/files" reloadable="true" crossContext="true"/>
```

完整配置

```
    <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <Context path="" docBase="pass" />
        <Context docBase="/data/files" path="/files" reloadable="true" crossContext="true"/>
```

按 ESC，输入 :wq 保存退出

## 启动与退出

1. 启动

```
// 进入目录
cd /usr/local/apache-tomcat-8.5.31/bin

./startup.sh
```
成功提示如下:
```
[root@tg-apps2 bin]# ./startup.sh
Using CATALINA_BASE:   /usr/local/apache-tomcat-8.5.31
Using CATALINA_HOME:   /usr/local/apache-tomcat-8.5.31
Using CATALINA_TMPDIR: /usr/local/apache-tomcat-8.5.31/temp
Using JRE_HOME:        /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre
Using CLASSPATH:       /usr/local/apache-tomcat-8.5.31/bin/bootstrap.jar:/usr/local/apache-tomcat-8.5.31/bin/tomcat-juli.jar
Tomcat started.
```

2. 退出

```
// 进入目录
cd /usr/local/apache-tomcat-8.5.31/bin

./shutdown.sh
```

3. 查看进程

```
ps -ef|grep java
```

启动成功就能看到进程

```
[root@tg-apps2 apache-tomcat-8.5.31]# ps -ef|grep java
root     15236     1  4 16:21 pts/1    00:00:02 /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java -Djava.util.logging.config.file=/usr/local/apache-tomcat-8.5.31/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -classpath /usr/local/apache-tomcat-8.5.31/bin/bootstrap.jar:/usr/local/apache-tomcat-8.5.31/bin/tomcat-juli.jar -Dcatalina.base=/usr/local/apache-tomcat-8.5.31 -Dcatalina.home=/usr/local/apache-tomcat-8.5.31 -Djava.io.tmpdir=/usr/local/apache-tomcat-8.5.31/temp org.apache.catalina.startup.Bootstrap start
root     15390  8634  0 16:22 pts/1    00:00:00 grep --color=auto java
```

如果使用 shutdown.sh 无法关闭进程，可以直接使用 kill -9 pid 的方法关闭
