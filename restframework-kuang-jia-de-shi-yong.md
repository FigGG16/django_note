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

##根据具体某个字段的值获取api
 - 在url中修改
 
```python
    #设置具体字段名，根据url的字段的值返回api
    url(r'^(?P<status>[\w-]+)/$', ArticleDetailAPIView.as_view(), name="detail"),

```
 - 在serializer.py文件中添加

```python
...
class ArticleDetailSerializers(ModelSerializer):
    class Meta:
        model = Article
        fields = [
            'title',
            'body',
            'created_time',
            'last_modified_time',
            'status',
            'abstract']
```

 - 在views中添加
 
```python
...
class ArticleDetailAPIView(RetrieveAPIView):

    queryset = Article.objects.all()
    serializer_class = ArticleDetailSerializers
    lookup_field = 'status'
```

![](/assets/Snip20171109_5.png)

##添加修改和删除
 - 包含头文件
 
```python
from  rest_framework.generics import (ListAPIView
                                      ,DestroyAPIView
                                      ,UpdateAPIView
                                      ,RetrieveAPIView
                                      )
```

 - 在View添加类

```python
#更新
class ArticleUpdateAPIView(UpdateAPIView):

    queryset = Article.objects.all()
    serializer_class = ArticleDetailSerializers
    lookup_field = 'status'


#删除
class ArticleDeleteAPIView(DestroyAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleDetailSerializers
    lookup_field = 'status'
...
```
 - url 
 
```
...
    #更新
    url(r'^(?P<status>[\w-]+)/edit/$', ArticleUpdateAPIView.as_view(), name="Update"),
    #删除
    url(r'^(?P<status>[\w-]+)/delete/$', ArticleDeleteAPIView.as_view(), name="Delete"),
```

##创建
 - 在serializer.py文件中添加类

```python
class ArticleCreateSerializers(ModelSerializer):
    class Meta:
        model = Article
        fields = '__all__'
```

 - 在view中包含框架和类

```python
from  rest_framework.generics import (***
                                      ,CreateAPIView
                                      )
创造
class ArticleCreateAPIView(CreateAPIView):
    queryset = Article.objects.all()
    serializer_class = ArticleCreateSerializers                                     
                                     
``` 

 - url
 

```python
    #创建
    url(r'^create/$',ArticleCreateAPIView.as_view(), name="create"),
```

##创建删除更新自定义值
![](/assets/Snip20171110_2.png)


##自定义权限
 - 首先在view中包含权限
 

```python
#导入权限
from rest_framework.permissions import (
            AllowAny,           #允许任何人
            IsAdminUser,        #是否登录
            IsAuthenticated,    #授权用户
            IsAuthenticatedOrReadOnly  #   只能读
)
```


 - 在view.py文件中需要权限的类中添加
```python
 permission_classes = [IsAuthenticated,IsAdminUser，...]
```


 - 用自定义的方式
 
 新建permissions.py文件并添加
 

```python
        #SAFE_METHODS   包含简单的请求方法
from rest_framework.permissions import BasePermission,SAFE_METHODS

class IsOwnerOrReadOnly(BasePermission):

    #GET则可以允许，PUT会遭到拒接请求
    my_safe_method = ['GET','PUT']
    message ='You must be the owner of this object.'
    def has_permission(self, request, view):
        if request.method in self.my_safe_method:
            return True
        return False
    
    #如果文章有用户字段的话此方法进行用户验证
    # def has_object_permission(self, request, view, obj):
    #     if request.method in SAFE_METHODS:
    #         return True
    #     return obj.user == request.user
```

 - 在view中导入该类
 

```
...
from .permissions import IsOwnerOrReadOnly


#在更新类中增加此权限
class ArticleUpdateAPIView(RetrieveUpdateAPIView):
    ...
    permission_classes = [... ,IsOwnerOrReadOnly]
    
```
拓展
>继承RetrieveUpdateAPIView，允许Allow: GET, PUT, PATCH, HEAD, OPTIONS
>继承UpdateAPIView ,Allow: PUT, PATCH, OPTIONS


 





