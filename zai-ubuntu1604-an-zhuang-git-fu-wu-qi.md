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





