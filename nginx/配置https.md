## Mac 下nginx本地https配置

1. 首先确保机器上安装了openssl和openssl-devel

   ```js
   npm install openssl
   npm install openssl-devel (安装报错 导致我没安装成功，但是也还是配置成功了 我感觉这个没啥用)
   ```

2. 给自己颁发证书

   /usr/local/etc/nginx/ (这个是证书的安装目录，建议放置在 nginx 根目录 )。

   openssl genrsa -des3 -out server.key 1024 (建立服务器私钥，在这个过程中需要输入密码短语，需要记住这个密码，后面会用到。 另：下面所有涉及到密码的都设置成了 123456, 也可以设置成自己的密码)

   ```js
   openssl req -new -key server.key -out server.csr
   ```

3. 上面一步执行完毕后，需要输入如下 内容 (填写超过三项即可)

   ```js
   Country Name(国家：中国填写CN)
   State or Province Name(区域或是省份：Beijing)
   Locality Name(地区局部名字：Beijing)
   Organization Name(机构名称：填写公司名)
   Organizational Unit Name(组织单位名称:部门名称)
   Common Name(网站域名)
   Email Address(邮箱地址)
   A challenge password(输入一个密码 反正我写的是 123456 )
   An optional company name(一个可选的公司名称)
   ```

4. 输入完这些内容，就会在当前目录生成server.csr文件， 顺序执行如下命令

   ```js
   cp server.key server.key.org
   openssl rsa -in server.key.org -out server.key
   openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
   ```

5. 配置Nginx配置 (记住：是在你需要进行https认证的域名的Nginx配置文件中 新增 一个server配置文件 )

   ```js
   server {
         server_name ezship.ezbuy.dev;
         listen      443 ssl;
         ssl_certificate     /usr/local/etc/nginx/server.crt;
         ssl_certificate_key /usr/local/etc/nginx/server.key;
   
         ssl_ciphers  HIGH:!aNULL:!MD5;
         ssl_prefer_server_ciphers  on;
   
         location / {
             proxy_pass http://127.0.0.1:9999;
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Real-PORT $remote_port;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         }
       }
   ```

6. 重启 Nginx

   ```js
   sodu nginx -s reload
   ```

7. 找到证书(/usr/local/etc/nginx/server.crt)，导入系统证书目录

8. 访问(访问的时候，如果提示不是私密链接，选择继续访问就好了)

前阵子更新了谷歌浏览器，访问公司产品的网站被拦了，不像以前可以直接点更多，然后点继续访问。网上有修改dns的方法，但是同事给我说了一个稍微快速一点的方法。

在图片图片所示的任何地方输入: thisisunsafe.没错就是这么6，然后就可以访问了。







-----

参考文档：

https://blog.csdn.net/weixin_42881744/article/details/101374310