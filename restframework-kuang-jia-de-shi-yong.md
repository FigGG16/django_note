##在项目目录小新建以下文件
![](/assets/Snip20171109_2.png)

 - 在serializer.py文件

```python
from  rest_framework.serializers import ModelSerializer
#导入自己的字段模型
from Category.models import Article
class ArticleSerializers(ModelSerializer):
    class Meta:
        model = Article
        #加载全部api
        fields='__all__'
        #自定义api
        #     [
        #     'title',
        #     'body',
        #     'created_time',
        #     'last_modified_time',
        #     'status',
        #     'abstract',
        #     'views',
        #     'likes',
        #     'topped',
        #     'category'
        #
        # ]
```


 - 在views文件中

```python
from  rest_framework.generics import ListAPIView,RetrieveAPIView
#包含模型
from Category.models import Article
#导入需要序列化的字段
from .serializers import ArticleSerializers

from rest_framework.response import Response
from django.http import HttpResponse

#显示api
class ArticleListAPIView(ListAPIView):
    #这两个是规定写法
    #获取模型对象
    queryset = Article.objects.all()
    #需要序列化的字段
    serializer_class = ArticleSerializers
```

 - 在urls

```Python
    #全部评论API
    url(r'^$', ArticleListAPIView.as_view(), name="json_api"),
```
##获取第几组的指定api

 - 在serializer.py文件中添加类

```python
...
class ArticleDetailAPIView(RetrieveAPIView):

    queryset = Article.objects.all()
    serializer_class = ArticleSerializers
```
 - urls

```python
...
    #需要指定第几组对象的api
    url(r'^(?P<pk>\d+)$', ArticleDetailAPIView.as_view(),name="detail"),
```

![](/assets/Snip20171109_4.png)





