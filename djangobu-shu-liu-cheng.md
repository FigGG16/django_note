##1操作系统选择
 - Ubuntu16.04
 
##搭建服务器环境
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