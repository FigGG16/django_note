激活

在urls添加

```py
from users.views import ActiveUserView
url(r'^active/(?P<active_code>.*)/$',ActiveUserView.as_view(),name="user_active")
```



在views里添加，验证处理类

```py
#邮箱验证模型
from .models import EmailVerifyRecord

#注册时调用，保存相应的内容
class ActiveUserView(View):
    def get(self,request,active_code):
        all_records = EmailVerifyRecord.objects.filter(code=active_code)
        if all_records:
            for record in all_records:
                email = record.email
                user = UserProfile.objects.get(email = email)
                user.is_active = True
                #在 LoginView 添加判断user.is_active:才能登录
                user.save()
        return render(request, "login.html")

```

邮想验证后处理，在登录类LoginView\(View\)添加判断，判断已验证则进行登录，否，提示激活

```py

 if user is not None:
                if user.is_active:
                    login(request, user)
                    return render(request, "index.html")
                else:
                    return render(request, "login.html", {"msg": "用户名未激活"})

```



