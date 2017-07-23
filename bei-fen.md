## xadmin的主题配置

主题配置效果如图

![](/assets/import3.png)

在user的adminx配置

```py
from  xadmin import views
class BaseSetting(object):
    enable_thems=True
    use_bootswatch = True

xadmin.site.register(views.BaseAdminView,BaseSetting)
```

全局配置

![](/assets/import4.png)

```py
class GlobalSetting(object):
    site_title = "幕学后台管理系统"
    site_footer = "暮学在线网"
    menu_style = "accordion"
xadmin.site.register(views.CommAdminView,GlobalSetting)
```

把英文换成中文

operation的apps.py添加

```
verbose_name=u"用户操作"
```

operation的\_\_init.\_py添加

```
default_app_config = "operation.apps.OperationConfig"
```

余下相同。

