#### 10.课程评论

1.加载评论页面

view

```py
class CommentsView(View):

    def get(self,request,course_id):
        course = Courses.objects.get(id=int(course_id))
        all_resources = CoursesResource.objects.filter(course=course)
        all_comments = CourseComments.objects.all()

        return render(request,"course-comment.html",{
            "course":course,
            'course_resourses':all_resources,
            'all_comments':all_comments
        })
```

urls

```py
url(r'comments/(?P<course_id>\d+)/$', CommentsView.as_view(), name="course_comments"),
```

实现跳转自己实现久好了

2.评论功能实现

view

```py
class AddComentsView(View):
    def post(self,request):
        if not request.user.is_authenticated():
            #没登录就返回json
            return HttpResponse('{"status":"fail","msg":"用户未登录"}',content_type='application/json')

        course_id = request.POST.get("course_id",0)
        comments = request.POST.get('comments',"")

        if int(course_id) >0 and comments:
            course_comments = CourseComments()
            #有多条数据或没有数据抛出异常
            course = Courses.objects.get(id=int(course_id))
            course_comments.course=course
            course_comments.comments=comments
            course_comments.user = request.user
            course_comments.save()
            return HttpResponse("{'status':'succes','msg':'添加成功'}", content_type='application/json')
        else:
            return HttpResponse("{'status':'file','msg':'添加失败'}", content_type='application/json')
```

URLS

```
    #添加课程评论，因为已经把参数放到POST中了不需要(?P<course_id>\d+)/
    url(r'comments/$', AddComentsView.as_view(), name="add_comments")
```

HTML

评论涉及ajax提交，看教撑

循环遍历评论内容



#### 1.全局搜索配置

配置课程的搜索，实现"或“的功能需要包含Q

```
from django.db.models import Q
```

```py
 search_keywords = request.GET.get('keywords',"")
        if search_keywords:
            # 课程搜索                              i代表不区分大小写
            all_courses = all_courses.filter(Q(name__icontains=search_keywords)|Q(desc__icontains=search_keywords)|Q(detail__icontains=search_keywor
```



