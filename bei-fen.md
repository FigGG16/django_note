注册时判断用户名是否存在

在RegisterView类里的post函数验证用户名

```py
if UserProfile.objects.filter(email=user_name):
    return render(request,"register.html",{"register_form":register_from, "msg":"用户已经存在"})
```



7.找回密码





