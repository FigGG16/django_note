#### 4.热门推荐

view

```
#热门课程推荐
hot_courses  = Courses.objects.all().order_by("-click_num")[:3]

return render(request,'course-list.html',{
/..../
    "hot_courses":hot_courses

})
```

html 

遍历

```
{% for hot_course in hot_courses %}
{% endfor %}
```

注意 

