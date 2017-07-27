7.提交学习咨询

因为form的字段跟form相似，直接用ModelForm转换即可





url分类\(在每个app下边建立自己的url文件\)

1.在organization新建urls文件

和主目录的样式一样

```
from django.conf.urls import url,include
from organization.views import OrgView

urlpatterns = [
    # 模版继承 --课程机构首页
    url(r'list/$', OrgView.as_view(), name="org_list"),
]
```

主目录url,include进来，然后加上命名空间

```
#课程机构url配置
    url(r'^org/', include('organization.urls',namespace="org")),

```



