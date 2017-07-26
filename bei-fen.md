##### 注册时判断用户名是否存在

##### 在RegisterView类里的post函数验证用户名

```py
if UserProfile.objects.filter(email=user_name):
    return render(request,"register.html",{"register_form":register_from, "msg":"用户已经存在"})
```

#### 7.找回密码

##### 先在forms添加类

```py
#找回密码
class ForgetForm(forms.Form):
    email= forms.EmailField(required=True)
    captcha = CaptchaField(error_messages={"invalid":u"验证码错误"})
```

##### 再在views类添加

```py
#找回密码
class ForgetPwdView(View):
    def get(self,request):
        forget_form = ForgetForm()
        return render(request,"forgetpwd.html",{"forget_form":forget_form })
   #post函数须另外配置
```

##### 配置url

```py
from users.views import ForgetPwdView
    #找回密码
url(r'forget/$', ForgetPwdView.as_view(), name="forget_pwd")
```

##### 处理重置密码的ur

##### 1.在forms添加

```py
#修改密码view
class ModifyPwdForm(forms.Form):
    password1 = forms.CharField(required=True, min_length=7)
    password2 = forms.CharField(required=True, min_length=7)
```

##### 2.在views里添加

```py
#重置密码
from .forms import ModifyPwdForm

class ModifyPwdView(View):
    def post(self, request):

        modify_form = ModifyPwdForm(request.POST)
        if modify_form.is_valid():
            pwd1 = request.POST["password1"]
            pwd2 = request.POST["password2"]

            email = request.POST["email"]
            if pwd1 != pwd2:
                return render(request, "password_reset.html", {"email": email, "msg": "密码不一致"})
            user = UserProfile.objects.get(email=email)
            user.password = make_password(pwd2)
            user.save()
            return render(request, "login.html")
        else:
            email = request.POST["email"]
            return render(request, "password_reset.html", {"email": email, "modify_form":modify_form})
```

##### url

```py
from users.views import ModifyPwdView
url(r'modify_pwd/$', ModifyPwdView.as_view(), name="modify_pwd"),
```

##### 注意 ：需在修改密码界面添加,以获取email,根据email修改密码

```py
<input type = "hidden" name="email" value = {{ email }}>
```



