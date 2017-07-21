 - operation models.py设计
 ![](/assets/Snip20170720_6.png)
 
 建用户咨询model
 ```python
 class UserAsk(models.Model):
    name = models.CharField(max_length=20,verbose_name=u"姓名")
    mobile = models.CharField(max_length=11,verbose_name=u"手机")
    course_name = models.CharField(max_length=50,verbose_name=u"课程名")
    add_time = models.DateTimeField(default=datetime.now,verbose_name=u"添加时间")

    class Meta:
        verbose_name=u"用户咨询"
        verbose_name_plural = verbose_name
 ```
  新建用户咨询
  ```python
class UserAsk(models.Model):
    name = models.CharField(max_length=20,verbose_name=u"姓名")
    mobile = models.CharField(max_length=11,verbose_name=u"手机")
    course_name = models.CharField(max_length=50,verbose_name=u"课程名")
    add_time = models.DateTimeField(default=datetime.now,verbose_name=u"添加时间")
  
  class Meta:
  verbose_name=u"用户咨询"
  verbose_name_plural = verbose_name

  ```
 新建课程评论，需要倒入用户的model和课程model的基本信息
 ```python
from users.models import  UserProfile
from courses.models import Courses
 
 ```
课程评论
```

```

 