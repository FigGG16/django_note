#### 8.机构详情页展示

因为课程没有一个外键指向机构，需要新增一个外键字段

在courses的models.py添加

```py
from organization.models import CourseOrg
course_org = models.ForeignKey(CourseOrg,verbose_name=u"课程机构",null=True,blank=True)
```

在views新建

```py
class OrgHomeView(View):
    """
    机构首页
    """
    def get(self,request,org_id):
        course_org = CourseOrg.objects.get(id=int(org_id))
        #有外键都可以用这种方法，反取出内容
        all_courses = course_org.courses_set.all()[:3]
        all_teacher = course_org.teacher_set.all()[:1]
        return render(request,"org-detail-homepage.html",{

            'all_courses':all_courses,
            'all_teacher':all_teacher,
            'course_org': course_org
            
        })
```

添加url

```py
url(r'^home/(?P<org_id>\d+)/$',OrgHomeView.as_view(),name="org_home")
```

跳转到机构列表

```py
 <a href="{% url 'org:org_home' course_org.id %}">
```

html循环遍历机构详情列表

```py
{% for course in all_courses %}
{% endfor %}
```



