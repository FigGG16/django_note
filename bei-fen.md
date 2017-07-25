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





