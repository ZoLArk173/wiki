# `apache` Web Server

## Change Root

1. edit `/usr/local/etc/apache24/httpd.conf`

   ```shell
   <Directory /home/zolark173/>
       Options Indexes FollowSymLinks
       AllowOverride All
       Require all granted
   </Directory>
   ```

- If you have set up virtual host, every `DocumentRoot` of virtual host must inside this folder. Otherwise you will get a permission error.

## Virtual Host

1. edit `/usr/local/etc/apache24/httpd.conf`

   ```shell
   # Virtual hosts
   Include etc/apache24/extra/httpd-vhosts.conf
   ```

2. edit `/usr/local/etc/apache24/extra/httpd-vhosts.conf`

   ```shell
   <VirtualHost *:80>
       ServerAdmin zolark173@60.nycu.cs
       DocumentRoot "/home/zolark173/ip/"
       ServerName 10.113.0.60
   </VirtualHost>
   
   <VirtualHost *:80>
       ServerAdmin zolark173@60.nycu.cs
       DocumentRoot "/home/zolark173/name/"
       ServerName 60.nycu.cs
   </VirtualHost>
   ```
   
3. restart `apache` server

## HTTPS

### Generate Certificate

1. install `openssh`

2. generate certificate

   ```shell
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /usr/local/etc/apache24/apache.key -out /usr/local/etc/apache24/apache.crt
   ```

### Enable HTTPS

1. edit `/usr/local/etc/apache24/httpd.conf`


   ```shell
   LoadModule ssl_module libexec/apache24/mod_ssl.so
   ...
   Include etc/apache24/extra/httpd-ssl.conf
   ```

2. edit `/usr/local/etc/apache24/extra/httpd-ssl.conf`

   ```shell
   <VirtualHost _default_:443>
       DocumentRoot "/home/zolark173/name"
       ServerName 60.nycu.cs:443
       ServerAdmin zolark173@60.nycu.cs
       SSLEngine on
       SSLCertificateFile "/usr/local/etc/apache24/apache.crt"
       SSLCertificateKeyFile "/usr/local/etc/apache24/apache.key"
       ...
   </VirtualHost>
   ```

### Auto Redirect to HTTPS

1. edit `/usr/local/etc/apache24/extra/httpd-vhosts.conf`

   ```shell
   <VirtualHost *:80>
       ...
       Redirect permanent / https://60.nycu.cs
       ...
   </VirtualHost>
   ```

2. restart `apache` server

### HSTS

HTTP Strict Transport Security

1. edit `/usr/local/etc/apache24/httpd.conf`

   ```shell
   LoadModule headers_module libexec/apache24/mod_headers.so
   ```

2. edit `/usr/local/etc/apache24/extra/httpd-vhosts.conf` and `/usr/local/etc/apache24/extra/httpd-ssl.conf`

   ```shell
   <VirtualHost *:80>
       ...
       Header always set Strict-Transport-Security "max-age=31536000;includeSubdomains; preload"
       ....
   </VirtualHost>
   ```

   31536000 is one year

3. restart `apache` server

## HTTP/2

1. edit `/usr/local/etc/apache24/httpd.conf`

   ```shell
   LoadModule http2_module libexec/apache24/mod_http2.so
   ```

2. edit `/usr/local/etc/apache24/extra/httpd-vhosts.conf` and `/usr/local/etc/apache24/extra/httpd-ssl.conf`

   ```shell
   <VirtualHost *:80>
       ...
       Protocols h2 http/1.1
       ....
   </VirtualHost>
   ```

3. restart `apache` server

# Web Page

- [apache - virtualhost-架設虛擬網站](https://mistech.pixnet.net/blog/post/91276198-apache---virtualhost-%E6%9E%B6%E8%A8%AD%E8%99%9B%E6%93%AC%E7%B6%B2%E7%AB%99)
- [How do I change the root directory of an Apache server?](https://stackoverflow.com/questions/5891802/how-do-i-change-the-root-directory-of-an-apache-server)
- [Ubuntu Apache 上用自簽憑證啟用 https 服務](https://ccnrz.wordpress.com/2017/05/04/%E5%9C%A8-ubuntu-apache-%E4%B8%8A%E5%95%9F%E7%94%A8-https-%E8%87%AA%E7%B0%BD%E6%86%91%E8%AD%89/)
- [Centos7,8設定 HSTS ( HTTP Strict Transport Security )](http://n.sfs.tw/content/index/15373)
- [HTTP/2 and PHP with Apache on FreeBSD](https://cromwell-intl.com/open-source/google-freebsd-tls/apache-http2-php.html)

