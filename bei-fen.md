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



