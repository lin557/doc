# Nginx 使用说明



## 安装

下载地址

```
http://nginx.org/en/download.html
```

### window

找到nginx/Windows-1.14.2下载到本地，解压。

### linux

1. 下载安装,  安装过程中会提示是否确认安装 ，选y(yes)

```
yum install nginx
```

2. yum无法安装nginx

```
# 缺少rpm包 执行以下命令后再执行 yum 就可以
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

3. 查看一下nginx安装在哪 

```
whereis nginx
```
```
[root@VM_157_185_centos ~]# whereis nginx
nginx: /usr/sbin/nginx /usr/lib64/nginx /etc/nginx /usr/local/nginx /usr/share/nginx /usr/share/man/man3/nginx.3pm.gz /usr/share/man/man8/nginx.8.gz
```
三个重要的目录
|       路径       |        作用         |
| :--------------: | :-----------------: |
| /usr/sbin/nginx  |    nginx启动路径    |
|    /etc/nginx    | 存放nginx的配置文件 |
| /usr/share/nginx |  默认的nginx资源库  |

3. 打开配置文件
```
cd /etc/nginx
// 没装vim 可以使用 vi 或安装yum install vim
vim nginx.conf
```
看看就可以，按Esc，输入:q 退出


## 启动与停止

1. 启动
```
// window
C:\server\nginx-1.14.2\start nginx
```
```
// linux
/usr/sbin/nginx
```

2. 停止
```
// window
C:\server\nginx-1.14.2\nginx -s stop
or
/usr/sbin/nginx -s quit
```

```
// linux
/usr/sbin/nginx -s stop
或
/usr/sbin/nginx -s quit
```
3. 重新载入nginx, 当配置信息修改，需要重新载入这些配置时使用此命令
```
// window
C:\server\nginx-1.14.2\nginx -s reload
```

```
// linux
/usr/sbin/nginx -s reload
```
4. 重新打开日志文件
```
// window
C:\server\nginx-1.14.2\nginx -s reopen
```

```
// linux
/usr/sbin/nginx -s reopen
```
5. 查看Nginx版本
```
// window
C:\server\nginx-1.14.2\nginx -v
```

```
// linux
/usr/sbin/nginx -v
```
6. 注册成服务
```
// window
可以使用Windows Service Wrapper包装成服务
```


linux 配置服务

```
vim /usr/lib/systemd/system/nginx.service
```
输入以下内容
```
[Unit]
Description=nginx - high performance web server 
Documentation=http://nginx.org/en/docs/
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx.conf
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
按 Esc, 输入:wq保存退出. 如果nginx已启动，先退出  /usr/sbin/nginx -s stop
```
// 配置开机启动
systemctl enable nginx.service
```
```
// 通过服务方式启动nginx
systemctl start nginx.service
```


## 配置

默认配置

```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

### 反向代理

1. 将访问80端口的请求代理到一个指定ip端口(192.168.0.250:8090)的服务上
```
http {
    # 参考默认配置, 略... 

    # 相关服务端口
    upstream traffic {
      server 192.168.0.250:8090;
    }

    server {
        listen       80;
        server_name  localhost;

        location / {
            # 这里最后加上 / 与不加 / 是有区别的     
            proxy_pass http://traffic/;
        }
    }
}
```

2. nginx反向代理proxy_pass url后加"/"与不加"/"的区别

```
# 当后面的url加上了/，相当于是绝对根路径，则nginx不会把location中匹配的路径部分代理走;如果没有/，则会把匹配的路径部分也给代理走。
# 例：访问路径为 /pss/bill.html

location /pss/ {
    # 当url带"/"时，代理到后端的路径为：http://127.0.0.1:18081/bill.html，省略了匹配到的/pss/路径；
    proxy_pass http://127.0.0.1:18081/;
}

location /pss/ {
    # 当url不带"/"时，代理到后端的路径为：http://127.0.0.1:18081/pss/bill.html，连同匹配到的/pss/路径，一起进行反向代理；
    proxy_pass http://127.0.0.1:18081;
}
```

3. 向被代理服务转换客户的真实ip与域名

```
    # 相关服务端口
    upstream traffic {
      server 192.168.0.250:8090;
    }
    
    server {
        listen       80;
        server_name  localhost;

        location / {
            # 这种配置情况下  192.168.0.250:8090 这台服务器上收到的url是 http://traffic/motor/xxxxx 不是真实的地址
            proxy_pass http://traffic/;
            
            # 需要加上以下配置转发真实ip与地址
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
```

4. 同域名下将不同的url跳转到不同的服务器上, 如 http://localhost/aa 跳转到192.168.0.250:8090, http://localhost/bb 跳转到 192.168.0.250:9999
```
http {
    # 参考默认配置, 略... 

    # 相关服务端口
    upstream traffic {
      server 192.168.0.250:8090;
    }
    
    upstream video {
      server 192.168.0.250:9999;
    }

    server {
        listen       80;
        server_name  localhost;

        # 注意这里 http://traffic/最后最好加上 /
        location /aa/ {
            proxy_pass http://traffic/;
        }
        
        # 注意结尾一定要加上 / 否则会打不开
        location /bb/ {
            proxy_pass http://video/;
        }
        
        # 可以直接使用域名地址
        location /cc/ {
            proxy_pass http://192.168.0.100:1234/;
        }        
        
        # location 后边的地址支持模糊匹配, 暂时没用到，略
    }
}
```

### 负载均衡

一个服务启用多个应用, 并负载使用
```
http {
    # 参考默认配置, 略... 

    # 相关服务端口
    upstream traffic {
      # ip_hash 用于实现会话保持功能，当某个客户端 多次请求定向到组内的一个服务器上，保证客户端与服务器建立稳定的会话。
      # ip_hash;
      server 192.168.0.250:8090;
      server 192.168.0.100:6060;
    }
    
    # 设置性能权重 默认为1 weight越大，负载的权重就越大
    #upstream traffic {
    #  server 192.168.0.250:8090 weight=2;
    #  server 192.168.0.100:6060 weight=1;
    #}

    server {
        listen       80;
        server_name  localhost;

        location / {
            proxy_pass http://traffic/;
        }
    }
}
```

### 启用gzip压缩

```
http {

    # 开启和关闭gzip模式
    gzip on;

    # gizp压缩起点，文件大于1k才进行压缩
    gzip_min_length 4k;

    # gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间
    gzip_comp_level 6;

    # 进行压缩的文件类型。
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/xml text/javascript application/json;

    # nginx对于静态文件的处理模块，开启后会寻找以.gz结尾的文件，直接返回，不会占用cpu进行压缩，如果找不到则不进行压缩
    # gzip_static on|off

    # 是否在http header中添加Vary: Accept-Encoding，建议开启
    gzip_vary on;

    # 设置压缩所需要的缓冲区大小，以4k为单位，如果文件为7k则申请2*4k的缓冲区 
    gzip_buffers 4 16k;

    # 设置gzip压缩针对的HTTP协议版本
    # gzip_http_version 1.1;

    # 设置反向代理的数据启用压缩 如果需要
    gzip_proxied any;

}
```

注意：电脑上装了ESET杀毒软件后，后自动帮助解压，在chrome 或 firefox 上都看不到压缩后的大小

Response Headers中不会出现

```
content-encoding: gzip
```

但会多出一行 

```
X-Content-Encoding-Over-Network: gzip
```

解决办法：设置ESET中的Web访问保护，将“启用应用程序协议内容过滤”功能设为禁用



### 启用SSL/https

```
    # HTTPS server
    
    server {
        listen       443 ssl;
        server_name  localhost;

        # 证书文件 *.crt *.cer
        ssl_certificate      /etc/nginx/ssl/xxx.crt;
        # 证书key文件
        ssl_certificate_key  /etc/nginx/ssl/xxx.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            root   html;
            index  index.html index.htm;
        }
    }
```



