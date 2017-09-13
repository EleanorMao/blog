---
title: Mac通过ssh免密登录Linux
date: 2017-09-12 16:15:38
pages: demo
---
1. `ssh-keygen -t rsa -C 'danningmao@outlook.com'`
2. `scp ~/.ssh/id_rsa.pub username@hostname:~/`
3. `ssh username@hostname`
4. `mkdir .ssh`(optional)
5. `cat id_rsa.pub >> .ssh/authorized_keys`
6. `vim ~/.ssh/config`
7. `Host        alias #自定义别名
    HostName        hostname  #替换为你的ssh服务器ip或domain
    Port            port #ssh服务器端口，默认为22
    User            user #ssh服务器用户名
    IdentityFile    ~/.ssh/id_rsa #第一个步骤生成的公钥文件对应的私钥文件`
8. `ssh alias #alias是你在~/.ssh/config文件配置的别名`