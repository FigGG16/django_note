# 后台管理系统的搭建

创建超级用户

```
createsuperuser
```

账号和密码

```
FigGG
admin123
```

遇到字段不符合，搜索并替换对应的字段值

```
makemigrations users#表名
```

修改后台为中文显示,时间与时区,setting配置

```
#中文
LANGUAGE_CODE = 'zh-hans'

#时区
TIME_ZONE = 'Asia/Shanghai'

#使用本地时间
USE_TZ = False
```

在users app 的admin.py注册 user\_profile

```py
rom .models import UserProfile

class UserProfileAdmin(admin.ModelAdmin):
    pass

admin.site.register(UserProfile,UserProfileAdmin)
```

## 问题1未解决：取消外键检查时，将不能添加账号

# 2.xadmin的安装\(直接看第2.5步即可\)

window

```python
sudo pip install -i https://pypi.doubanio.com/simple/ --trusted-host pypi.doubanio.com xadmin
```

mac安装要使用github的最新版本

```python
pip install git+git://github.com/sshwsfc/xadmin.git
```

2.1注册配置setting App

```
    'xadmin',
    'crispy_forms'
```

2.2修改urls

```py
import xadmin

urlpatterns = [
    url(r'^xadmin/', xadmin.site.urls),

]
```

2.3注释掉users 的admin.py默认的admin注册方法

2.4当访问xadmin时会提示 “1146, "Table 'mxonline.xadmin\_usersettings' doesn't exist"“,创建表

```
makemigrations
migrate
```

2.5  'set' object is not reversible 解决：把大括号换成中括号

```py
urlpatterns = [

    url(r'^xadmin/', xadmin.site.urls),
    # url(r'^xadmin/', xadmin),
]
```

2.5下载xadmin的原码，paste到项目文件目录，新建extra\_app目录,把xadmin放到里面.最后source Root extra\_app文件夹！ 把环境变量的xadmin的卸载掉。

2.6配置setting文件

```py
sys.path.insert(0,os.path.join(BASE_DIR,'extra_apps'))
```

# 3. users app的model注册

在users文件夹新建adminx.py文件,打开并添加以下内容，效果图所示

![](/assets/Snip20170722_4.png)

```py
import xadmin

from .models import EmailVerifyRecord

class EmailVerifyRecordAdmin(object):
    #邮箱显示列
    list_display = ['code', 'email', 'send_type', 'send_time']
    #搜索功能
    search_fields = ['code', 'email', 'send_type']
    #筛选功能
    list_filter = ['code', 'email', 'send_type', 'send_time']
   # pass

xadmin.site.register(EmailVerifyRecord,EmailVerifyRecordAdmin)
```

修复

![](/assets/Snip20170722_2.png)

```py
    def __str__(self):
        return '{0}({1})'.format(self.code,self.email)
```

#### 3.1，注册轮播图\(以上一样\)

```py
class BannerAdmin(object):

    list_display = ['title', 'image', 'url', 'index','add_time']

    search_fields =['title', 'image', 'url', 'index']

    list_filter =['title', 'image', 'url', 'index','add_time']

xadmin.site.register(EmailVerifyRecord,EmailVerifyRecordAdmin)
xadmin.site.register(Banner,BannerAdmin)
```

#### 3.2添加课程，然后添加章节时出现下效果，重写\_\_str\_\_方法即可

![](/assets/import.png)

3.3通过外键搜索课程的章节

![](/assets/import2.png)

\#

# 4xadmin的管理页面配置

#### 4.1主题配置效果如图

![](/assets/import3.png)

#### 在user的adminx配置

```py
from xadmin import views
class BaseSetting(object):
enable_thems=True
use_bootswatch = True

xadmin.site.register(views.BaseAdminView,BaseSetting)
```

#### 全局配置

![](/assets/import4.png)

```py
class GlobalSetting(object):
site_title = "幕学后台管理系统"
site_footer = "暮学在线网"
menu_style = "accordion"
xadmin.site.register(views.CommAdminView,GlobalSetting)
```

#### 把英文换成中文

operation的apps.py添加

```
verbose_name=u"用户操作"
```

operation的\_\_init.\_py添加

```
default_app_config = "operation.apps.OperationConfig"
```

#### 余下相同。



