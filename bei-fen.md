 - 授课机构的model设计
 ![](/assets/Snip20170720_5.png)
 新建课程机构App organization
 创建城市model
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