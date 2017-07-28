#### 

实现下图功能点击

0.1实现点击机构课程

![](/assets/importHome.png)

View

```py
class OrgCourseView(View):
    """
    机构课程列表页
    """
    def get(self,request,org_id):
        current_page = "course"
        course_org = CourseOrg.objects.get(id=int(org_id))
        #有外键都可以用这种方法，反取出内容
        all_courses = course_org.courses_set.all()
        return render(request,"org-detail-course.html",{

            'all_courses':all_courses,
            'course_org':course_org,
            'current_page':current_page
        })
```

urls

```py
from organization.views import OrgCourseView
url(r'^course/(?P<org_id>\d+)/$',OrgCourseView.as_view(),name="org_course")
```

html循环遍历机构课程的全部课程遍历都是老油条啦\)

```
{% for course in all_courses %}
{% endfor %}
```

0.2实现点击机构介绍



