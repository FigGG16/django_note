##1-客户端

修改本地的hosts文件
```
sudo vi /etc/hosts
```
![](/assets/Snip20171114_1.png)

尝试着ping一下

```
ping gitserver
```
##2-服务器

安装openssh-server

```
sudo apt-get install openssh-server
```


 - 2-1这布操作是禁止使用ssh登录服务器，如不需要，可跳过
```
sudo vi /etc/ssh/sshd_config 
```
文本输入

```
/PasswordA
```
回车
![](/assets/Snip20171114_2.png)
保存编辑，重启ssh

```
sudo service sshd restart
```
好了。以上操作将不能使用ssh登录，也不能使用FTP传送文件


添加git账户,并切换到git账户

```
sudo git adduser git
```
在该用户的目录下，新建.ssh文件夹，并修改权限为700,然后在该目录下新建authorized_keys文件并设置600权限

```
& mkdir .ssh
& chmod 700 .ssh
& touch .ssh/authorized_keys
& chmod 600 .ssh/authorized_keys
```

##3客户端
添加允许访问git服务器仓库的ssh证书
找到客户端的ssh证书

```
cd ~/.ssh
```
![](/assets/Snip20171114_3.png)

##4服务器
拷贝到git账户服务器目录，再把证书添加到.ssh/authorized_keys

```
& cat id_rsa.pub >> ~/.ssh/authorized_keys

```
此时返回到客户端使用 ssh git@gitserver 还不能够访问.

继续安装git

```
& sudo apt-get install git
```

 - 4-1 使用git-shell 限制客户端ssh登录当期用户(不需要可跳过)

找到git-shell的目录

```
which git-shell
```
输出：/usr/bin/git-shell   拷贝

然后编辑shells

```
& sudo vi /etc/shells 
```
![](/assets/Snip20171114_5.png)

更改默认启动shell的路径

```
sudo chsh git
```
![](/assets/Snip20171114_6.png)

返回客户端尝试进行ssh登录就会报错
```
hint: ~/git-shell-commands should exist and have read and execute access.
```
好了！ 配置git-shell 就结束了！


 - 4-2继续
 在指定目录下新建git文件夹
 
```
$ sudo mkdir git
$ cd git/
#创建仓库文件夹
$ sudo mkdir project-name.git
$ cd project-name.git
#实例仓库
$ sudo git init --bare
```
设置git 目录的访问权限组

```
sudo chown -R git:git git
```

4-2：实现自动同步到站点目录

进入裸仓库：/home/testgit/project-name.git


```
$ cd /home/testgit/sample.git
$ cd hooks
//这里我们创建post-receive文件
$ vim post-receive
//在该文件里输入以下内容
#!/bin/bash
git --work-tree=/home/项目目录 checkout -f
//保存退出后，将该文件用户及用户组都设置成git
$ chown git:git post-receive
//由于该文件其实就是一个shell文件，我们还应该为其设置可执行权限
$ chmod +x post-receive

```


##5客户端

安装git

```
sudo apt-get install git
```
配置git全局属性


```
$ git config --global user.name "name"
$ git config --global user.email xxx@xxx.com
$ git config --global core.editor gedit

```

选择指定目录初始化仓库

```
git init
```
然后新增文件

```
$ touch README
```
添加到仓库的暂存区

```
$ git add .
```
提交到本地仓库

```
$ git commit -m"***"
```

添加远程仓库

```
git remote add origin git@gitserver:~/git/project-name.git
```

提交到远程仓库

```
git push origin master
```

好了！  安装结束！


























