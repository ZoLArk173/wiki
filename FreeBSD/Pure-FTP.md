# `pure-ftpd` FTP Server

## TLS

Add FTP over TLS encryption support.

1. ### Install OpenSSL
   
   ```shell
   sudo pkg install openssl
   ```

2. ### Configure `pure-ftpd`
   
    Open `pure-ftpd.conf`.
   
   ```shell
   vim /usr/local/etc/pure-ftpd.conf
   ```
   
    Set `TLS` to `1` or `2`.
   
   ```shell
   [...]
   # This option can accept three values :
   # 0 : disable SSL/TLS encryption layer (default).
   # 1 : accept both traditional and encrypted sessions.
   # 2 : refuse connections that don't use SSL/TLS security mechanisms,
   #     including anonymous sessions.
   # Do _not_ uncomment this blindly. Be sure that :
   # 1) Your server has been compiled with SSL/TLS support (--with-tls),
   # 2) A valid certificate is in place,
   # 3) Only compatible clients will log in.
   
   TLS                      2
   [...]
   ```

3. ### Creating The SSL Certificate For TLS
   
   To use TLS, we must create an SSL certificate.
   
   ```shell
   mkdir -p /etc/ssl/private/
   openssl req -x509 -nodes -days 7300 -newkey rsa:2048 -keyout /etc/ssl/private/pure-ftpd.pem -out /etc/ssl/private/pure-ftpd.pem
   ```
   
   Change the permissions of the SSL certificate.
   
   ```shell
   chmod 600 /etc/ssl/private/pure-ftpd.pem
   ```
   
   And then restart `pure-ftpd`.
   
   ```shell
   sudo /usr/local/etc/rc.d/pure-ftpd restart
   ```

## Anonymous User

To allow anonymous to access ftp server, system **must** have an user called `ftp`.

```shell
pw groupadd ftpuser -g 21  
pw useradd ftp -u 21 -g ftpuser -c "Anonymous FTP user" -d /home/ftp -s /sbin/nologin  
mkdir /home/ftp  
chown ftp:ftpuser /home/ftp
```

## Webpage

- [雄的 FreeBSD 筆記‧FTP 的架設─pure-ftpd](http://mail.lsps.tp.edu.tw/~gsyan/freebsd2001/ftp-pureftpd.html)
- [anonymous FTP broken with pureftpd](https://forums.gentoo.org/viewtopic-t-283647-start-0.html#:~:text=%22To%20run%20an%20anonymous%20FTP%20server%20you%20must%20have%20a%20*system*%20account%20called%20%27ftp%27.%20Don%27t%20give%20it%20any%20valid%20shell%2C%20just%20a%20home%20directory.%20That%20home%20directory%20is%20the%20anonymous%20area.%22)
- [How To Configure PureFTPd To Accept TLS Sessions On CentOS 6.2 (howtoforge.com)](https://www.howtoforge.com/how-to-configure-pureftpd-to-accept-tls-sessions-on-centos-6.2)

# `pure-ftp` Upload Script

Run a script when someone upload a file to ftp server.

## Usage

```shell
pure-uploadscript [-p </path/to/pidfile>] [-B] [-g <gid>] [-h] -r <program to run> [-u <uid>]
```

- `-B`
  - Daemonize the process and fork it in background.
- `-r <program to run>`
  - What program to run. `$PATH` is ignored. It has to be an absolute filename.
  - `pure-uploadscript` will run the program with **an absolute upload filename as the first argument** (`$1`). 

## Environment Variable

- `UPLOAD_SIZE`
  - size of the file, in bytes.
- `UPLOAD_PERM`
  - permission
- `UPLOAD_UID`
- `UPLOAD_GID`
- `UPLOAD_USER`
  - username
- `UPLOAD_GROUP`
- `UPLOAD_VUSER`
  - virtual username

## Webpage

- https://www.systutorials.com/docs/linux/man/8-pure-uploadscript/