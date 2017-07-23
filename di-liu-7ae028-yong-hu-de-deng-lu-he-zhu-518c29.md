备注：参考第三章导入静态文件

6.1首页和登录页面的配置

urls简单的登录界面页面\(配置静态文件参考第三章\)

```py
from django.views.generic import TemplateView

url('^$',TemplateView.as_view(template_name="index.html"),name="index")
```

配置setting引用

```py
STATICFILES_DIRS= [
    os.path.join(BASE_DIR,'static')
]
```

链接样式和图片

```py
<link rel="stylesheet" type="text/css" href="/static/css/reset.css">
```

登录界面也同理,添加url

```py
url('^login/$',TemplateView.as_view(template_name="login.html"),name="login")
```

实现登录页面跳转



