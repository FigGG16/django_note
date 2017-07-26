##### 1。模版继承

新建 一个base.html拷贝org\_list.html内容到里面, 把org\_\_list.html清空当成子继承

把需要继承重写的部分给包住，

```py
{% block title %}......{% endblock %}
```

需要自定义css

```py
{% block custom_css %}   {% endblock %}
```

需要自定义js

```py
{% block custom_js %} {% endblock %}
```

重置标题

```
{% block custom_bread %}
{% endblock %}
```

重置内容

```
{% block content %}
{% endblock %}
```

在继承的html模板只要把包含的内容拷贝到里面就可以了！ 可以进行修改，等等！



加载课程详情的url

在organization的views添加

```py
from django.views.generic import View
class OrgView(View):
    def get(self,request):
        return render(request,"org-list.html",{})
```

urls配置

```
from organization.views import OrgView

 #模版继承 --课程机构首页
 url(r'org_list/$', OrgView.as_view(), name="org_list"),

```



