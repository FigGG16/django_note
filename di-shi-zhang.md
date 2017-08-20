#### 1.  修改头像

在Form配置类

```py
# 文件上传
class UploadImageForm(forms.ModelForm):
    class Meta:
        model = UserProfile
        fields = ['image']
```

在View中

```py
from django.http import HttpResponse
class UploadImageView(LoginRequiredMixin,View):
    """
    用户修改头像
    """
    def post(self,request):
        image_form =UploadImageForm(request.POST,request.FILES,instance=request.user)
        if image_form.is_valid():
            image = image_form.cleaned_data['image']
            request.user.save()
            return HttpResponse('{"status":"success"}', content_type='application/json')
        else:
            return HttpResponse('{"status":"fail"}', content_type='application/json')
```

urls

```py
url(r'^image/upload/$', UploadImageView.as_view(), name="image_upload"),
```

HTML ,修改头像是一个post请求，

```py
 <form class="clearfix" id="jsAvatarForm" enctype="multipart/form-data" autocomplete="off" method="post" action="{% url 'users:image_upload' %}" target='frameFile'>
```

#### 2.小喇叭显示未读消息数量

在user的模型中添加获取消息函数

```py
def get_unread_nums(self):
    #获取未读消息数量
    from operation.models import UserMessage #必须把这个MODEL放到这里，放到开头就是循环引用的
```



django小知识

1. 在下标为1的基础上加2递增

```
{{ forloop.counter|add:2 }} 
```

2.   计数到5时变为0

```
{% if forloop.counter|divisibleby:5 %}
```



