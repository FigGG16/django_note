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
from django.shortcuts import render
from django.contrib.auth import authenticate,login

# Create your views here.

def user_login(request):
    if request.method =="POST":
        #user_name=request.POST.get("username","")
        #pass_word=request.POST.get("password","")
        user_name=request.POST['username']
        pass_word=request.POST['password']
        user = authenticate(username=user_name,password=pass_word)

        if user is not None:
            login(request,user)
            return render(request,"index.html")
        else:
            return render(request,"login.html",{})
        pass
    elif request.method =="GET":
        return render(request,"login.html",{})
```

在urls添加

```py
from users.views import user_login

url('^login/$',login,name="login")
url('^login/$',user_login,name="login")
```

登录后防止CSRF攻击，在&lt;/form&gt;表单的最后加上

```
{%csrf_token%}
```

在index.html,加入django语法判断是否是登录状态

```py
{% if request.user.is_authenticated %}
    #已经登录执行的代码
    {% else %}
    #还未登录时执行的代码
{% endif %}
```

#### 通过自定义auth完善邮箱\(和用户名\)登录

在user的 Views.py里添加

```py
from django.contrib.auth.backends import ModelBackend
from django.db.models import Q

from .models import UserProfile

class CustomBackend(ModelBackend):
    def authenticate(self, username=None, password=None, **kwargs):
        try:
            user = UserProfile.objects.get(Q(username=username)|Q(email=username))
            if user.check_password(password):
                 return user
        except Exception as e:
            return None
```

在setting中添加

```py
AUTHENTICATION_BACKENDS=(
    'users.views.CustomBackend',
)
```

## 6.4用from\(类\)实现登录

在user新建forms.py文件并添加

```py
from django import forms

class LoginForm(forms.Form):
    #判断密码是否存在，指定长度
    username=forms.CharField(required=True)
    password = forms.CharField(required=True,min_length=5)
```

view添加以下

```py
from django.views.generic.base import View
from .forms import LoginForm

class LoginView(View):
    def get(self,request):
        return render(request,"login.html",{})

    def post(self,request):
        #声明一个form实例
        login_form =LoginForm(request.POST)
        if login_form.is_valid():
            user_name = request.POST['username']
            pass_word = request.POST['password']
            user = authenticate(username=user_name, password=pass_word)

            if user is not None:
                login(request, user)
                return render(request, "index.html")
            else:
                return render(request,"login.html",{"msg": "用户名或密码错误"})
        else:
            return render(request, "login.html", {"login_form":login_form})
```

urls配置

```py
from users.views import LoginView
url('^login/$',LoginView.as_view(),name="login")
```

界面优化

![](/assets/importLogin.png)

在对应的输入框的div添加属性

```py
{% if login_form.errors.username %}  errorput {% endif %}
```

提示框

```py
{% for key, error in login_form.errors.items%}{{ error }}{% endfor %}{{ msg }}
```



