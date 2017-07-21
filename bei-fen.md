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
```python
class CourseComments(models.Model):
    "课程评论"
    user = models.ForeignKey(UserProfile,verbose_name=u"用户")
    course= models.ForeignKey(Courses,verbose_name=u"课程")
    comments = models.CharField(max_length=200,verbose_name=u"评论")
    add_time = models.DateTimeField(default=datetime.now, verbose_name=u"添加时间")
    
    class Meta:
        verbose_name = u"课程评论"
        verbose_name_plural = verbose_name
```
用户收藏
```python
class UserFavorite(models.Model):
    user = models.ForeignKey(UserProfile, verbose_name=u"用户")
    course = models.ForeignKey(Courses, verbose_name=u"课程")
    fav_id = models.IntegerField(default=0,verbose_name=u"数据id")
    degree = models.CharField(choices=(("1", "课程"), ("2", "课程机构"), ("3", "讲师")),default=1,verbose_name=u"收藏类型")
    add_time = models.DateTimeField(default=datetime.now, verbose_name=u"添加时间")
    
    class Meta:
        verbose_name = u"用户收藏"
        verbose_name_plural = verbose_name
```
用户消息
```python
class UserMessage(models.Model):
    #消息分为两种，一：定向发给某一个user 二：系统发给所有人的消息
    #0->发给所有人信息 ，如果时等于用户id的据取出来
    user = models.IntegerField(default=0,verbose_name=u"接受用户")
    message=models.CharField(max_length=500,verbose_name=u"消息内容")
    has_read = models.BooleanField(default=False,verbose_name=u"是否已读")
    add_time = models.DateTimeField(default=datetime.now, verbose_name=u"添加时间")
    
    class Meta:
        verbose_name= u"用户消息"
        verbose_name_plural=verbose_name
```
用户学习课程
```python
  #CourseComments model比这个多了个评论。可以使用继承的方式
class UserCourse(models.Model):
    user = models.ForeignKey(UserProfile, verbose_name=u"用户")
    course = models.ForeignKey(Courses, verbose_name=u"课程")
    add_time = models.DateTimeField(default=datetime.now, verbose_name=u"添加时间")
    
    class Meta:
        verbose_name= u"用户课程"
        verbose_name_plural=verbose_name
```






 