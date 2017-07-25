6.7用户的注册

views

```py
class RegisterView(View):
    def get(self,request):
        return render(request,"register.html",{})
```

urls

```py
from users.views import RegisterView
url('^register/$',RegisterView.as_view(),name="register")
```

html跳转

```py
<a style="color:white" class="fr registerbtn" href="{% url 'register' %}">注册</a>
```

快捷更换加载静态文件目录方法







