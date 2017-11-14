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

 










