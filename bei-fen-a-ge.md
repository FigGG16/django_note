课程列表展示

拷贝CCOURSE\_LIST.HTML到template,配置include courses URL

```
#课程列表url配置
url(r'^course/', include('courses.urls',namespace="course")),
```

使用继承,继承内部内容即可，继承后的course-list.html的文件内容如下

```py
{#声明继承的类#}
{% extends 'base.html' %}

{% block title %} 公开课列表页 {% endblock %}

{#加载静态文件目录#}
{% load staticfiles %}

{#面包板#}
{% block custom_bread %}
<section>
/...../
</section>
{% endblock %}

{#内容部分#}
{% block content %}
<section>
/...../
</section>
{% endblock %}
```

view

```py
from django.views.generic import View
from django.shortcuts import render

class CourseListView(View):
    def get(self,request):
    
    all_courses = Courses.objects.all()
    return render(request,'course-list.html',{
                "all_courses":courses,
            })
```

urls

```py
from django.conf.urls import url,include
from .views import CourseListView

urlpatterns = [
    # 模版继承 --课程机构首页
    url(r'list/$', CourseListView.as_view(), name="course_list"),
]
```

html

```
{% for course in all_courses %}
{% endfor %}
```

分页

CourseListView类中添加

```py
try:
    page = request.GET.get('page', 1)
except PageNotAnInteger:
    page = 1
p = Paginator(all_courses, 3, request=request)
courses = p.page(page)
```

html实现复制之前的就好了





