1.建好django项目  
2.新建静态目录存在css html js 和图片文件  
3.新建日记文件  
4.用户的上传文件目录  
5.新建app文件家  
6.生成app

![](/assets/Snip20170717_2.png)

输入框写入

```
startapp messqge
```

7.把app生成文件夹拖到app文件夹

8.mark source  app文件夹

## 1.页面的搭建（从请求到响应的完整流程）

#### 1.1导入html文件  
复制留言板到-&gt;template-&gt;新建css文件—&gt;剪切留言板.html的style样式到css文件，-&gt;然后在留言板.html文件里链接css文件

```html
    <link rel="stylesheet" href="/static/css/style.css">
```

####2.1设计数据库

 - 2.1.1

![](/assets/Snip20170718_1.png)
 - 2.1.2
 ![](/assets/Snip20170718_2.png)
该怎么解决？<br/>

安装mysql驱动 在window下安装
```
pip install mysql-python
```
在mac下安装
```
pip install pymysql
```

####3.1在setting文件里配置数据表

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME':"testdjango",
        "USER":"root",
        "PASSWORD":"root",
        "HOST":"127.0.0.1"
    }
}
```
生成数据表指令
![](/assets/Snip20170718_3.png)

#### 4.1.导入html文件到django文件夹的urls
在app文件的view.py里写

```python
def getform(request):     #要返回的html文件名
    return render(request,'message_form.html')
```
urls
```python
from django.conf.urls import url
from django.contrib import admin
from message.views import getform

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^form/$',getform)
]
```

配置setting.py的DIRS文件夹

```python
'DIRS': [os.path.join(BASE_DIR, 'templates')]
```
然后设置加载样式(在setting指明样式文件夹的路径)
```python
STATICFILES_DIRS= [
    os.path.join(BASE_DIR,'static')
```
![](/assets/Snip20170718_4.png)

## 2.django的orm和model数据模型的编写

####1. 把操作数据库当成类
在app文件夹的model.py文件夹写上
```python
class UserMessage(models.Model):
    name = models.CharField(max_length=20,verbose_name="用户名")
    email = models.EmailField(verbose_name=u"邮箱")
    address = models.CharField(max_length=100,verbose_name=u"联系地址")
    message = models.CharField(max_length=500,erbose_name=u"留言信息")
    class Meta:
        verbose_name="用户留言信息"
```
在setting中注册app

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 添加
    'message'
]
```
创表指令
![](/assets/Snip20170719_5.png)


##3.数据表的增删改查

在views里倒入创建模型函数
```python
#.代表和views同一目录下的文件
from .models import UserMessage
```
在def getform(request): 里添加获取方法
```python
    all_messages=UserMessage.objects.all()
    for message in all_messages:
        print (message.name)
```

form表单提交一定要加上

```python
    {%  csrf_token %}
```
数据库的增加操作
```python
    if request.method == "POST":
        name = request.POST.get('name','')
        message=request.POST.get('name','')
        address=request.POST.get('address','')
        email=request.POST.get('email','')
        user_message = UserMessage()
        user_message.name=name
        user_message.message=message
        user_message.address=address
        user_message.email=email
        user_message.object_id="hellow1d3"
        user_message.save()
    return render(request, 'message_form.html')
```
数据库的删除
``` python   
#all_message = UserMessage.objects.filter(name='bobby',address='北京')
#all_message.delete()
```
数据库的获取
```python
    # all_messages=UserMessage.objects.all()
    # for message in all_messages:
    #     print (message.name)
```


##4.django的url配置，以及将数据库的内容呈现到html页面上
4.1把数据库的内容显示到html
 - 在views.py里加上
 ```python
   message = None
    all_messages = UserMessage.objects.filter(name='黄飞翔')
    if all_messages:
        message = all_messages[0]

    return render(request, 'message_form.html',{
        "my_message":message
    })
 ```
 - 在对应的标签加上属性value="\{{ my_message.name }}"
 
 ```html
 <input id="name" type="text" value="{{ my_message.name }}" name="name" class="error" placeholder="请输入您的姓名"/>
 ```
 - 唯一要注意的就是textarea标签的位置不一样
 ```html
  <textarea id="message" name="message"  placeholder="请输入你的建议">{{ my_message.message }}</textarea>
 ```

建立

