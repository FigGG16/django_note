#### 配置404

404，500页面加载静态文本目录的配置

1.stting

```
STATIC_ROOT =os.path.join(BASE_DIR,'static')
```

2.

```py
DEBUG = False #必须=False，不加载STATICFILES_DIRS，需自己配置静态文本目录

#当DEBUG = False  ALLOWED_HOSTS = ['*'] 代表链接地址，所有地址均可
ALLOWED_HOSTS = ['*']
```

3.view

```py
def page_nont_found(request):
    #全局404处理函数
    from django.shortcuts import render_to_response
    response = render_to_response('404.html',{})
    response.status_code=404
    return response
```

4.URL

```py
from mxOnline.settings import STATIC_ROOT


url(r'static/(?P<path>.*)$', serve, {"document_root": STATIC_ROOT}),
```

全局

```py
#配置全局404页面
handler404 = 'users.views.page_nont_found'
handler500 = 'users.views.page_error'
```



