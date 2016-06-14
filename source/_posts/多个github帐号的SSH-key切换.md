---
title: 多个github帐号的SSH key切换
date: 2016-03-24 08:59:41
tags:
    - git
    - SSH key
---
一台电脑上有一个ssh key，在github上提交代码，由于其他原因
你可能会在一台电脑上提交到不同的github上，怎么办呢...

假设你电脑上一个ssh key都没有,如果有默认的一个了，请直接生成第二个
<!-- more -->
### 一、生成并添加第一个ssh key
``` bash
$ ssh-keygen -t rsa -C "youremail@xxx.com"
```

在Git Bash中执行命令一路回车，会在~/.ssh/目录下生成id_rsa和id_rsa.pub两个文件

用文本编辑器打开id_rsa.pub里的内容，在Github中添加SSH Keys

### 二、生成并添加第二个ssh key
``` bash
$ ssh-keygen -t rsa -C "youremail@xxx.com"
或者 指定生成目录及文件名
$ ssh-keygen -t rsa -f /tmp/id_rsa.sohu -C "youremail@xxx.com"
```

如果未指定目录，则这次不要一路回车了，给这个文件起一个名字 不然默认的话就覆盖了之前生成的第一个

### 三、在.ssh/下创建config文件 内容如下：
```
    Host github.com  
        HostName github.com  
        PreferredAuthentications publickey  
        IdentityFile ~/.ssh/id_rsa  
      
    Host my.github.com  
        HostName github.com  
        IdentityFile ~/.ssh/my 

    Host oschina.net
        HostName git.oschina.net
        IdentityFile ~/.ssh/id_rsa 
```

Host名字随意，接下来会用到。

### 四、测试配置是否正确
``` bash
$ ssh -T git@github.com
```
如果出现Hi xxx!You've successfully authenticated 就说明连接成功了

### 五、现在就以下种情况给出不同的做法：

* 本地已经创建或已经clone到本地：

    如下两种解决方法：

    打开.git/config文件

    ``` bash
    #更改[remote "origin"]项中的url中的  
    #my.github.com 对应上面配置的host  
    [remote "origin"]  
        url = git@my.github.com:itmyline/blog.git 
    ```

    或者在Git Bash中提交的时候修改remote 

    ``` bash
    $ git remote rm origin  
    $ git remote add origin git@my.github.com:itmyline/blog.git
    ```

* clone仓库时对应配置host对应的账户
    ``` bash
    #my.github.com对应一个账号  
    git clone git@my.github.com:username/repo.git  
    ```