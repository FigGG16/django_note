备注：参考第三章导入静态文件

6.1首页和登录页面的配置

urls简单的登录界面页面\(配置静态文件参考第三章\)

```py
from django.views.generic import TemplateView

url('^$',TemplateView.as_view(template_name="index.html"),name="index")
```



