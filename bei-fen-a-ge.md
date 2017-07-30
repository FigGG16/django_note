7.课程章节信息

view

```py
class CourseInfoView(View):
    """
    课程章节信息
    """
    def get(self,request,course_id):
        course = Courses.objects.get(id=int(course_id))
        return render(request,"course-video.html",{
            "course":course,
        })
```

urls

```
url(r'info/(?P<course_id>\d+)/$', CourseInfoView.as_view(), name="course_info")
```

\*\*\*\*

8.资源下载

在后端添加好资源

view中

```py
class CourseInfoView(View):
    """
    课程章节信息
    """
    def get(self,request,course_id):
        course = Courses.objects.get(id=int(course_id))
        all_resources = CoursesResource.objects.filter(course=course)
        return render(request,"course-video.html",{
            "course":course,
            'course_resourses':all_resources
        })

```

html循环

```js
  {% for course_resource in course_resourses %}
      <li>
          <span ><i class="aui-iconfont aui-con-file"></i>&nbsp;&nbsp;{{ course_resource.name }}</span>
          <a href="{{ MEDIA_URL }}{{ course_resource.download }}" class="downcode" target="_blank" download="" data-id="274" title="">下载</a>
      </li>
  {% endfor %}
```

就能实现下载了

