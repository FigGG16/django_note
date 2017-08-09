执行

```
pip install -i https://pypi.doubanio.com/simple/ djangoUeditor
```

注册

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

在xadmin添加插件

在以下目录兴建一个uediyor.py文件

![](/assets/importUeditor.png)



