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







