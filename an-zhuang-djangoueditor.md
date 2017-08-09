## 1.python2执行

```
pip install -i https://pypi.doubanio.com/simple/ djangoUeditor
```

##### python3下载DjangoUeditor3的zip,然后

```
cd ...目录。。。。。
```

##### workon进入虚拟环境

```
python setup.py install
```

## 2.注册

```
"DjangoUeditor"
```

urls

```
   #富文本相关
    url(r'^ueditor/', include('DjangoUeditor.urls'))
```

在course添加model

```
from DjangoUeditor.models import UEditorField
```

重写字段

```
 detail=UEditorField(u'课程详情',width=600, height=300, imagePath="courses/ueditor/", filePath="courses/ueditor/",default='')
```

在course的adminx.py添加

```
style_fields = {"detail":"ueditor"}
```

## 3.在xadmin添加插件

在以下目录兴建一个uediyor.py文件

![](/assets/importUeditor.png)

加上通用代码

```py
import xadmin
from django.db.models import TextField
from xadmin.views import BaseAdminPlugin, ModelFormAdminView, DetailAdminView
from DjangoUeditor.models import UEditorField
from DjangoUeditor.widgets import UEditorWidget
from django.conf import settings


class XadminUEditorWidget(UEditorWidget):
    def __init__(self,**kwargs):
        self.ueditor_options=kwargs
        self.Media.js = None
        super(XadminUEditorWidget,self).__init__(kwargs)

class UeditorPlugin(BaseAdminPlugin):
    def get_field_style(self, attrs, db_field, style, **kwargs):
        if style == 'ueditor':
            if isinstance(db_field, UEditorField):
                widget = db_field.formfield().widget
                param = {}
                param.update(widget.ueditor_settings)
                param.update(widget.attrs)
                return {'widget': XadminUEditorWidget(**param)}
            if isinstance(db_field, TextField):
                return {'widget': XadminUEditorWidget}
        return attrs
    def block_extrahead(self, context, nodes):
        js = '<script type="text/javascript" src="%s"></script>' % (settings.STATIC_URL + "media/ueditor/ueditor.config.js")
        js += '<script type="text/javascript" src="%s"></script>' % (settings.STATIC_URL + "media/ueditor/ueditor.all.min.js")
        nodes.append(js)


xadmin.site.register_plugin(UeditorPlugin, DetailAdminView)
xadmin.site.register_plugin(UeditorPlugin, ModelFormAdminView)
```



