备注：参考第三章导入静态文件

## 6.1首页和登录页面的配置

#### urls简单的登录界面页面\(配置静态文件参考第三章\)

```py
from django.views.generic import TemplateView
#emplate_nam自动识别html的路径
                                                                #name的名字随意
url('^$',TemplateView.as_view(template_name="index.html"),name="index")
```

#### 配置setting引用

```py
STATICFILES_DIRS= [
                         #存放静态文件的目录    
    os.path.join(BASE_DIR,'static')
]
```

#### 链接样式和图片

```py
<link rel="stylesheet" type="text/css" href="/static/css/reset.css">
```

#### 登录界面也同理,添加url

```py
url('^login/$',TemplateView.as_view(template_name="login.html"),name="login")
```

#### 实现登录页面跳转

```html
                                           直接指定跳转的html文件即可
<a style="color:white" class="fr loginbtn" href="/login/">登录</a>
```

## 6.3完成用户登录的后台逻辑

django自动添加request函数，当我们配置的url，django就会自动生成一个reques放到函数里面。

在users文件里的views.py添加

```py
def login(request):
    if request.method =="POST":
        pass
    elif request.method =="GET":
        return render(request,"login.html",{})
```

在urls添加

```py
from users.views import login

url('^login/$',login,name="login")
```

登录后防止CSRF攻击，在&lt;/form&gt;表单的最后加上

```
{%csrf\_token%}
```


