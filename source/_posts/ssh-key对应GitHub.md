---
title: ssh-key对应GitHub
date: 2018-05-31 20:36:46
tags: Jszy
categories: [随笔]
---

### 本地配置全局user 

首先保证本地已经配置了git用户，全局配置命令如下：

    git config --global user.name 'Jszy'
    git config --globlal user.email 'xxx@gmail.com'


### 生成一个自定义的ssh  key

    # 新建ssh key
    cd ~/.ssh
    ssh-keygen -t rsa -C 'XXX@gmail.com'
    # 设置一个自定义名词，例如：id_rsa_two
    Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): id_rsa_two

然后打开`.ssh`文件夹就可以发现生成了两个新的东西，`id_rsa_two` 和 `id_rsa_two.pub`，把公钥添加到对应的github账户key setting中......还没完...

### 新建config文件

需要在`.ssh`文件夹下新建一个`config` 文件（相当于一个映射文件），内容如下：

```
# 该文件用于配置私钥对应的服务器
# 默认github账户
  Host github.com
  HostName github.com
  User git
  IdentityFile C:/Users/user/.ssh/id_rsa

# 另一个github账户
# 建一个github别名，新建的帐号使用这个别名做克隆和更新
  Host github2
  HostName github.com
  User git
  IdentityFile C:/Users/user/.ssh/id_rsa_two
```

然后就可以分别测试一下两个ssh能否通信：

```
    # github.com
    $ ssh -T git@github.com
    Hi Jszy! You've successfully authenticated, but GitHub does not provide she
    ll access.
    # github2
    $ ssh -T github2
    Hi Jszy! You've successfully authenticated, but GitHub does not provide she
    ll access.
```

### 最后一步

设置具体的`config`文件，正常在`.git`文件中：

    url = git@github.com:Jszy/test.git #修改前
    url = github2:Jszy/test.git #修改后

github2字样和上面的设置是对应的。很显然，我们需要引导它去读取正确的私钥去和服务端保存的公钥匹配。因为示例是的两个服务端都是github，如果是别的服务器的话，则保存服务器的域名就可以了，比如一个是 GitHub，一个是 GitLab，那么就不用再设置这一步了，会去自动匹配服务器，关联私钥。
