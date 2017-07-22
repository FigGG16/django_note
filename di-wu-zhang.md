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

问题1未解决：取消外键检查时，将不能添加账号



