# 数据库的设计


![](/assets/Snip20170720_2.png)



## 1.设计userApp的model(通过自定义的user去覆盖django默认的user)

 - ####1.1新建app

```
startapp users
```
 - 1.2 打开models文件添加继承auth_user表字段
 ```python
 from django.contrib.auth.models import AbstractBaseUser
 ```
 - 1.3 创建字段
 ```python
 class UserProfile(AbstractUser):
    nick_name = models.CharField(max_length=50,verbose_name=u"昵称",default=" ")
    birday = models.DateField(verbose_name=u"生日",null=True,blank=True)
    gender=models.CharField(choices=(("male",u"男") ,("female","女")),default="female")
    address=models.CharField(max_length=100,default=u"")
    mobile = models.CharField(max_length=11,null=True,blank=True)
    image = models.ImageField(upload_to="image/%Y/%m",default=u"image/default.png",max_length=100)
    #配置表的Meta信息
    class Meta:
        verbose_name="用户信息"
        verbose_name_plural=verbose_name
    #重载unicode方法，防止print的时候不能打印字符串
    def __unicode__(self):
        return self.username
 ```
 - 1.4 setting注册app
 ```
  'users'
 ```
 - 1.5setting重载setting方法
 ```
 ROOT_URLCONF = 'mxOnline.urls'
 ```
 - 创建表
 ```
 makemigrations users
 
 ```
## 2.userModel的完善(解决循环引用问题)
![](/assets/Snip20170720_1.png)
 - 2.1分成设计
 ![](/assets/Snip20170720_3.png)
 
 - 2.2完善邮箱验证码和轮播图的model
  ```python
  class EmailVerifyRecord(models.Model):
    #以下两个都不能为空
    code = models.CharField(max_length=20,verbose_name=u"验证码")
    email=models.EmailField(max_length=50,verbose_name=u"邮箱")
    
    #找回密码时的验证码
    send_type = models.CharField(choices=(("register",u"注册") ,("forget",u"找回密码")),max_length=10)
    send_time = models.DateTimeField(default=datetime.now)
    
    class Meta:
        verbose_name=u"邮箱验证码"
        verbose_name_plural=verbose_name
        
  class Banner(models.Model):
    title= models.CharField(max_length=100,verbose_name="标题")
    image=models.ImageField(upload_to="banner/%Y/%m",verbose_name=u"轮播图",max_length=100)
    url = models.URLField(max_length=200,verbose_name=u"访问地址")
    index = models.IntegerField(default=100,verbose_name=u"顺序")
    add_time=models.DateTimeField(default=datetime.now,verbose_name=u"添加时间")
    
    class Meta:
        verbose_name = u"轮播图"
        verbose_name_plural=verbose_name
  ```
 - ####3.3新建coursesApp
 models如下
 ![](/assets/Snip20170720_4.png)<br/>
 
  ```python
 class Courses(models.Model):
    name= models.CharField(max_length=50,verbose_name=u"课程名")

    desc = models.CharField(max_length=300,verbose_name=u"课程描述")
    detail=models.TextField(verbose_name=u"课程详情")
    degree = models.CharField(choices=(("cj","初级"),("zj","中级"),("gj","高级")),max_length=2)
    learn_times = models.IntegerField(default=0,verbose_name=u"学习时长(分钟数)")
    students=models.IntegerField(default=0,verbose_name=u"学习人数")
    fav_nums=models.IntegerField(default=0,verbose_name=u"收藏人数")
    image = models.ImageField(upload_to="courses/%Y/%m", verbose_name=u"封面图", max_length=100)
    click_num = models.IntegerField(default=0,verbose_name=u"点击数")
    add_time = models.DateTimeField(default=datetime.now,verbose_name=u"添加时间")

    class Meta:
        verbose_name=u"课程";
        verbose_name_plural = verbose_name
        
   ```
   因为一个课程对应多个章节，存在一对多的关系，所以要使用django外健的形式完成,django没有多对一，一对多的映射
   ```python
   class Lesson(models.Model):
    course= models.ForeignKey(Courses,verbose_name=u"课程")
    name=models.CharField(max_length=100,verbose_name=u"章节名")
    add_time = models.DateTimeField(default=datetime.now, verbose_name=u"添加时间")
   ```
   添加视频
   ```python
   class Video(models.Model):
    lesson=models.ForeignKey(Lesson,verbose_name=u"章节")
    name = models.CharField(max_length=100,verbose_name=u"视频名")
    add_time=models.DateTimeField(default=datetime.now, verbose_name=u"添加")

    class Meta:
        verbose_name = u"视频"
        verbose_name_plural=verbose_name
   ```
   添加课程资源
   ```python
   class CoursesResource(models.Model):
    course = models.ForeignKey(Courses, verbose_name=u"课程")
    name=models.CharField(max_length=100,verbose_name=u"名称")
    download=models.FileField(upload_to="course/resource/%Y/%m",verbose_name=u"资源文件",max_length=100)
    add_time = models.DateTimeField(default=datetime.now, verbose_name=u"添加时间")

    class Meta:
        verbose_name = u"课程资源"
        verbose_name_plural=verbose_name
   ```
 - ####3.4授课机构的model设计
 ![](/assets/Snip20170720_5.png)
 
 新建课程机构App organization
 创建城市model(外键)
 ```python
 class CityDict(models.Model):
    name=models.CharField(max_length=20,verbose_name=u"城市")
    desc = models.CharField(max_length=200,verbose_name=u"描述")
    add_time = models.DateTimeField(default=datetime.now)

    class Meta:
        verbose_name = u"城市"
        verbose_name_plural=verbose_name
 ```
 课程机构基本信息
 ```python
 class CourseOrg(models.Model):
    name = models.CharField(max_length=50,verbose_name=u"机构名称")
    desc = models.TextField(verbose_name=u"机构名称")
    click_nums=models.IntegerField(default=0,verbose_name=u"点击数")
    fav_nums = models.IntegerField(default=0,verbose_name=u"收藏数")
    image = models.ImageField(upload_to="org/%Y/%m",verbose_name=u"封面图", max_length=100)
    address = models.CharField(max_length=150,verbose_name=u"机构地址")
    #在课程列表页可以通过地址筛选，所以定义城市外键
    city = models.ForeignKey(CityDict,verbose_name=u"所在城市")
    add_time = models.DateTimeField(default=datetime.now)
    
    class Meta:
        verbose_name = u"课程机构"
        verbose_name_plural=verbose_name
 ```
 教师基本信息
 ```python
 class Teacher(models.Model):
    org = models.ForeignKey(CourseOrg,verbose_name=u"所属机构")
    name = models.CharField(max_length=50, verbose_name=u"教师名")
    work_years=models.IntegerField(default=0,verbose_name=u"工作年限")
    work_company=models.CharField(max_length=50,verbose_name=u"就职公司")
    work_position = models.CharField(max_length=50,verbose_name=u"公司职位")
    points = models.CharField(max_length=50,verbose_name=u"教学特点")
    click_nums=models.IntegerField(default=0,verbose_name=u"点击数")
    fav_nums = models.IntegerField(default=0,verbose_name=u"收藏数")

    class Meta:
        verbose_name = u"教师"
        verbose_name_plural=verbose_name
 ```
 - ####3.5operation models.py设计
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
   add_time =  models.DateTimeField(default=datetime.now,verbose_name=u"添加时间")
   class Meta:
   verbose_name=u"用户咨询"
   verbose_name_plural = verbose_name

 ```
新建课程评论，需要倒入用户的model和课程model的基本信息
```python
from users.models import UserProfile
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
最后添加注册app
```
"courses",
"operation",
"organization"
``` 