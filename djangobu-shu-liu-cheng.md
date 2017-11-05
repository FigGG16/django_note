##1操作系统选择
 - Ubuntu16.04
 
##搭建服务器环境
 - 添加用户
 ```
 adduser myName
 ```
 - 1安装git
 ```
 sudo apt-get undata
 sudo apt-get install git
 ```
 - 2先安装python多版本管理工具pyenv
 ```python
 $ git clone git://github.com/yyuu/pyenv.git ~/.pyenv
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc
$ exec $SHELL -l
 
 ```
 - 3查看可以安装的python版本：
 ```
 pyenv install --list
 ```
 - 4必须要安装python所需要的依赖包
 ```python
 $ sudo apt-get install libc6-dev gcc
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm
 ```
 - 5上面的依赖包搞定之后，我们就可以安装python了：
 ```python
 $ pyenv install 3.4.3 -v
 ```
 - 6设置全局的python版本
 ```python
 $ pyenv global 3.4.3
 $ pyenv versions
     system
   * 3.4.3 (set by /home/seisman/.pyenv/version)
 
 ```
 
 - 7pip3安装virtualenv和virtualenvwrapper
 
 ```
 sudo pip install virtualenv
 sudo pip install virtualenvwrapper
 ```
 
 - 8新建虚拟环境和workon全局变量
 ```
 8.1$   mkvirtualenv  myBokeEnv
 8.2$   vim ~/ .baserc
 ```
  - 到该文件的末尾处添加，并屏蔽 if~fi 间的内容
  ![](/assets/20150421001110213.jpeg)
  ```python
  if [ -f /usr/local/bin/virtualenvwrapper.sh ]; then
    export WORKON_HOME=$HOME/.virtualenvs
    source /usr/local/bin/virtualenvwrapper.sh
  fi
  ```

 - 9按照追梦人物的博客严格部署，注意的是：部署项目的环境与开发环境所使用的插件要完全相同，不然会有一堆错误，几个常用的快捷键
 ```
  nginx -s reload  ：修改配置后重新加载生效
  nginx -s stop  :快速停止nginx
  start nginx   启动Nginx  
 ```
##数据库的操作
 - 1安装数据库
 ```Python
  1. sudo apt-get install mysql-server

  2. apt-get isntall mysql-client

  3.  sudo apt-get install libmysqlclient-dev 
 ```
 - 2建表并指明utf-8字符集 
 ```sql
 CREATE DATABASE newboke
 DEFAULT CHARACTER SET utf8
 DEFAULT COLLATE utf8_general_ci;
 ```
## 通过以上总结
1. 服务器有错误要开启debug模式调试
2. django的过滤函数要重写到一个文件,不能用环境变量中的文件，更新维护太麻烦
3. 还没有把ssh加密上
4. git要学好分支管理，方便更新服务器上的项目


