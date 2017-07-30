##### 6.动态页面更新\(接受较为特殊的\)

view

```

```

动态更新图片

```py
<img width="440" height="445" src="{{ MEDIA_URL }}{{ course.image }}" class="jqzoom" />
```

动态更新章节数\(因为并没有定义章节数字段\)，解决办法，在Courses  mode中定义一个函数

```py
#获取课程章节数
def get_zj_nums(self):
    return self.lesson_set.all().count()
```

html访问调用

```py
<li><span class="pram word3">章&nbsp;节&nbsp;数：</span><span>{{{ course.get_zj_nums }}}</span></li>
```

这种 方法的缺点是不能传递字段

学习人数和章节数一样

```py
def get_learn_user(self):
    return self.usercourse_set.all()[:5]
```

html

```js
{% for users_course  in course.get_learn_user%}

<span class="pic"><img width="40" height="40" src="{{ MEDIA_URL }}{{ users_course.user.image }}"/></span>

{% endfor %}

```



