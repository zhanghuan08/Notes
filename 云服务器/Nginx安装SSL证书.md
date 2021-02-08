## Nginx安装SSL证书

**1、将从控制台下载的SSL证书的两个文件上传到nginx目录下的cret目录中,没有目录自行创建**

![image-20200617120808071](http://club.codehuan.cn/images/image-20200617120808071.png)

**2、打开 Nginx 安装目录下 conf 目录中的 nginx.conf 文件，找到：**

```shell
# HTTPS server
# #server {
# listen 443;
# server_name localhost;
# ssl on;
# ssl_certificate cert.pem;
# ssl_certificate_key cert.key;
# ssl_session_timeout 5m;
# ssl_protocols SSLv2 SSLv3 TLSv1;
# ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
# ssl_prefer_server_ciphers on;
# location / {
#
#
#}
#}
```

**3、将其修改为  :**

```shell
 server {
     listen       443 ssl;
     listen       [::]:443 default_server;
     server_name  www.codehuan.cn; #填写绑定证书的域名
     root         /usr/share/nginx/html;
     #证书文件名称
     ssl_certificate  cert/www.codehuan.cn.pem;
     #私钥文件名称
     ssl_certificate_key cert/www.codehuan.cn.key;
     ssl_session_timeout 5m;
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
     ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
     ssl_prefer_server_ciphers on;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
    proxy_pass http://59.110.173.13:8080;
    }

    error_page 404 /404.html;
    location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
  }
 server {
        listen 80;
        #填写绑定证书的域名
        server_name www.codehuan.cn;
        #把http的域名请求转成https
        return 301 https://$host$request_uri;
  }

```

**4、重启nginx**

```shell
systemctl reload nginx
```

![image-20200617122018002](http://club.codehuan.cn/images/image-20200617122018002.png)