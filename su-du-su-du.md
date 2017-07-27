3.列表分页功能

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



