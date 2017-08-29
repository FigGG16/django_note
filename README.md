# mac系统自带python2.7,所以下载并安装了python3

## 先安装pip

```
sudo easy_install pip
```

### 再安装virtualenv

```
sudo pip install virtualenv
```

### 然后安装virtualenvwrapper

```
sudo pip install virtualenvwrapper
```

### 需要注意的是我们安装了virtualenvwrapper，不能像window那样直接调用virtualenvwrapper的，需要设置他的环境变量。

### 首先查找virtualenvwrapper.sh文件放在哪

```
sudo find / -name virtualenvwrapper.sh
```

### 会出现搜到 /usr/local/bin/virtualenvwrapper.sh

### 打开环境变量

```
vim ~/.bash_profile
```

### 在文中底下加入如下

```
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

### OK

### 新建环境变量py2web和py3web

```
mkvirtualenv py2web
```

###因为py3web用的是python3所以要指定路径

```
mkvirtualenv --python=/Users/kinggg/py3/bin/python py3web
```
ok  .

###<font color="red"> 最后使用workon 进入环境变量。。。。。就这样</font>

### 配置完交了很多智商税！










