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

注意：

当取出课程难度时，因为字段的保存类型为, 不能显示中文

```py
degree = models.CharField(verbose_name=u"难度",choices=(("cj","初级"),("zj","中级"),("gj","高级")),max_length=2)
```

用另外一种来获取

```js
<span class="fl">难度：<i class="key">{{ hot_course.get_degree_display }}</i></span>
```



