#### 8.机构详情页

因为课程没有一个外键指向机构，需要新增一个外键字段

在courses的models.py添加

```
from organization.models import CourseOrg
course_org = models.ForeignKey(CourseOrg,verbose_name=u"课程机构",null=True,blank=True)
```



