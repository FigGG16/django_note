5.授课机构的排行

在view中添加

```py
#热门机构的提取
hot_orgs = all_orgs.order_by("-click_nums")[:3]
```

html

![](/assets/importJGPM.png)



6.排序

新增字段

```py
students = models.IntegerField(default=0,verbose_name=u"学习人数")
course_nums = models.IntegerField(default=0,verbose_name=u"课程数")
```

html

```py
<li class="{% if sort == '' %}active{% endif %}"><a href="?&ct={{ category }}&city={{ city_id }}">全部</a> </li>
<li class="{% if sort == 'students' %}active{% endif %}"><a href="?sort=students&ct={{ category }}&city={{ city_id }}">学习人数 &#8595;</a></li>
<li class="{% if sort == 'courses' %}active{% endif %}"><a href="?sort=courses&ct={{ category }}&city={{ city_id }}">课程数 &#8595;</a></li>
```

view

```py
sort = request.GET.get('sort',"")
if sort:
    if sort =="students":
        all_orgs=all_orgs.order_by("-students")

    elif sort =="courses":
        all_orgs = all_orgs.order_by("-course_nums")

```



