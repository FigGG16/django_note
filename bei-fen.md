#### 1。模版继承

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

#### 在继承模版中进行自定义设置

![](/assets/importCustomPirtue.png)

#### 2.课程机构列表页展示1

后台添加资源目录存放的配置

新建media文件夹

在setting配置

1.

```py
#指明静态资源目录路径
MEDIA_URL='/media/'
MEDIA_ROOT =os.path.join(BASE_DIR,'media')
```

2.

![](/assets/importPirtue.png)

配置上传的文件处理函数

```py
from django.views.static import serve
from mxOnline.settings import MEDIA_ROOT

url(r'media/(?P<path>.*)$',serve,{"document_root":MEDIA_ROOT})
```

在html中循环遍历即可

```py
{% for   city in all_citys %}
{% endfor %}
```

再然后设置加载图片的路径

```py
<img width="200" height="120" class="scrollLoading" data-url="{{ MEDIA_URL }}{{ course_org.image }}"/>
```

在organization的views添加

```py
class OrgView(View):
    def get(self,request):
        all_orgs = CourseOrg.objects.all()
        org_nums = all_orgs.count()
        all_citys = CityDict.objects.all()
        return render(request,"org-list.html",{
            "all_orgs":all_orgs,
            "all.citys":all_citys,
            "org_nums":org_nums
        })
```

URLs中

```py
#madie
from django.views.static import serve
from mxOnline.settings import MEDIA_ROOT

#配置文件
    url(r'media/(?P<path>.*)$',serve,{"document_root":MEDIA_ROOT})
```

#### 3.列表分页功能

下载插件

```
sudo pip install django-pure-pagination
```

添加到注册apps

```py
INSTALLED_APPS = (
    ...
    'pure_pagination',
)
```

在html中使

1.view的设置

```py
class OrgView(View):
    def get(self,request):
        all_orgs = CourseOrg.objects.all()
        org_nums = all_orgs.count()
        all_citys = CityDict.objects.all()
        #-------
        #对课程机构进行分页
        try:
            page = request.GET.get('page', 1)
        except PageNotAnInteger:
            page = 1
        p = Paginator(all_orgs,3, request=request)
        orgs = p.page(page)

        #_________
        return render(request,"org-list.html",{
            "all_orgs":orgs,
            "all.citys":all_citys,
            "org_nums":org_nums
        })
```

2.在html中添加object\_list

```py
{% for course_org in all_orgs.object_list %}
```

3.参考官方文档

```html
            <ul class="pagelist">

                {% if all_orgs.has_previous %}
                   <li class="long"><a href="?{{ all_orgs.previous_page_number.querystring }}">上一页</a></li>
                {% endif %}
                {% for page in all_orgs.pages %}
                    {% if page %}
                        {% ifequal page all_orgs.number %}
                            <li class="active"><a href="?{{ page.querystring }}">{{ page }}</a></li>
                        {% else %}
                             <li><a href="?{{ page.querystring }}" class="page">{{ page }}</a></li>
                        {% endifequal %}
                    {% else %}
                        <li class="none"><a href="">...</a></li>
                    {% endif %}
                {% endfor %}
                {% if  all_orgs.has_next %}
                    <li class="long"><a href="?{{ all_orgs.next_page_number.querystring }}">下一页</a></li>
                {% endif %}

            </ul>
```



