##### 6.动态页面更新

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



