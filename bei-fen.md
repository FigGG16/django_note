注册时判断用户名是否存在

在RegisterView类里的post函数验证用户名

```py
if UserProfile.objects.filter(email=user_name):
    return render(request,"register.html",{"register_form":register_from, "msg":"用户已经存在"})
```

7.找回密码

先在forms添加类

```py
#找回密码
class ForgetForm(forms.Form):
    email= forms.EmailField(required=True)
    captcha = CaptchaField(error_messages={"invalid":u"验证码错误"})
```

再在views类添加

```
#找回密码
class ForgetPwdView(View):
    def get(self,request):
        forget_form = ForgetForm()
        return render(request,"forgetpwd.html",{"forget_form":forget_form })
    def post(self,request):
        forget_form = ForgetForm(request.POST)
        if forget_form.is_valid():
            email = request.POST["email"]
            send_register_email(email, "forget")
            return render(request, "send_success.html")
        else:
            return render(request, "forgetpwd.html", {"forget_form": forget_form})


```



